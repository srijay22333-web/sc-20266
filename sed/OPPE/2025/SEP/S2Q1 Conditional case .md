# Conditional Case Conversion using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* For lines tagged `low`: extract content after `:` and convert to **lowercase**
  — but only if **no lowercase letter already exists** in the content
* For lines tagged `up`: extract content after `:` and convert to **uppercase**
  — but only if **no uppercase letter already exists** in the content
* If the target case is already present → keep the content exactly as-is

---

## 📥 Sample Input

```text
low : THIS IS LOW
up: This is UP
```

---

## 📤 Sample Output

```text
this is low
This is UP
```

---

## ✅ Solution — CLI

```bash
sed '
/^low/ {
  s/.*: //
  /[a-z]/! s/.*/\L&/
}
/^up/ {
  s/.*: //
  /[A-Z]/! s/.*/\U&/
}
' input.txt
```

## ✅ Solution — script.sed

```sed
/^low/ {
  s/.*: //
  /[a-z]/! s/.*/\L&/
}
/^up/ {
  s/.*: //
  /[A-Z]/! s/.*/\U&/
}
```

```bash
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### Logic breakdown — before writing a single command

```
Line: "low : THIS IS LOW"
  ├── tagged "low"  → enter low block
  ├── extract content after ": " → "THIS IS LOW"
  ├── any lowercase [a-z] present? → NO
  └── convert to lowercase → "this is low"  ✓

Line: "up: This is UP"
  ├── tagged "up"   → enter up block
  ├── extract content after ": " → "This is UP"
  ├── any uppercase [A-Z] present? → YES (T, U, P)
  └── keep as-is → "This is UP"  ✓
```

---

### 🔹 Command Block — `/^low/ { }`

```bash
/^low/ {
  ...commands...
}
```

* `/^low/` → selects lines starting with `low`
* `{ }` → groups both commands so they run **only on low-tagged lines**
* Lines tagged `up` never enter this block and vice versa

---

### 🔹 Extract Content — `s/.*: //`

```bash
s/.*: //
```

* `.*` → greedily matches everything from start
* `: ` → matches the colon + space separator
* Together `.*: ` is deleted → only the content after it remains

```
"low : THIS IS LOW"  →  s/.*: //  →  "THIS IS LOW"
"up: This is UP"     →  s/.*: //  →  "This is UP"
```

> Note: This handles both `low : ` (space before colon) and `up: ` (no space
> before colon) because `.*` absorbs everything up to the last `: ` sequence.

---

### 🔹 Guard Condition — `/[a-z]/!` and `/[A-Z]/!`

```bash
/[a-z]/! s/.*/\L&/    ← for low block
/[A-Z]/! s/.*/\U&/    ← for up block
```

* `/[a-z]/` → matches if ANY lowercase letter exists in the pattern space
* `!` → negation — the command only runs if the condition is **false**
* So `/[a-z]/! s/.*/\L&/` means: **"convert to lowercase ONLY IF no lowercase letter is present"**

```
"THIS IS LOW"   →  /[a-z]/  →  no lowercase found  →  ! fires  →  CONVERT  ✓
"this is low"   →  /[a-z]/  →  lowercase found      →  ! skips  →  KEEP     ✓
"This is UP"    →  /[A-Z]/  →  uppercase found      →  ! skips  →  KEEP     ✓
"this no upper" →  /[A-Z]/  →  no uppercase found   →  ! fires  →  CONVERT  ✓
```

---

### 🔹 Case Conversion — `\L&` and `\U&`

```bash
s/.*/\L&/    ← convert entire line to lowercase
s/.*/\U&/    ← convert entire line to uppercase
```

* `.*` → matches the entire pattern space
* `\L` → convert everything that follows to **lowercase** (until `\E` or end)
* `\U` → convert everything that follows to **uppercase** (until `\E` or end)
* `&` → backreference to the **entire match** (the whole line)
* Combined: `\L&` = lowercase the entire matched content

```
"THIS IS LOW"  →  s/.*/\L&/  →  "this is low"
"this no upper" →  s/.*/\U&/  →  "THIS NO UPPER"
```

> `\L` and `\U` are **replacement-side** special tokens — they only work
> inside the replacement part of `s/pattern/replacement/`.
> They are not regex — they control case of the output.

---

### 🔹 Full Execution Trace — All 4 Test Cases

```
Input: "low : THIS IS LOW"
  /^low/ → MATCH → enter block
  s/.*: // → "THIS IS LOW"
  /[a-z]/! → no lowercase → fires
  s/.*/\L&/ → "this is low"
  Output: this is low  ✓

Input: "low : this is already low"
  /^low/ → MATCH → enter block
  s/.*: // → "this is already low"
  /[a-z]/! → lowercase EXISTS → skips
  Output: this is already low  ✓ (kept as-is)

Input: "up: This is UP"
  /^up/ → MATCH → enter block
  s/.*: // → "This is UP"
  /[A-Z]/! → uppercase EXISTS → skips
  Output: This is UP  ✓ (kept as-is)

Input: "up: this has no uppercase"
  /^up/ → MATCH → enter block
  s/.*: // → "this has no uppercase"
  /[A-Z]/! → no uppercase → fires
  s/.*/\U&/ → "THIS HAS NO UPPERCASE"
  Output: THIS HAS NO UPPERCASE  ✓
```

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Regex address | `/^low/` | Selects lines starting with "low" |
| 2 | Command block | `{ }` | Groups commands under one address |
| 3 | Strip prefix | `s/.*: //` | Removes everything up to and including `: ` |
| 4 | Character class guard | `/[a-z]/!` | Fires only when NO lowercase letter exists |
| 5 | Negation | `!` | Inverts the address condition |
| 6 | Full-line match | `s/.*/` | Matches entire pattern space content |
| 7 | Lowercase conversion | `\L&` | Converts entire matched content to lowercase |
| 8 | Uppercase conversion | `\U&` | Converts entire matched content to uppercase |
| 9 | Whole match reference | `&` | Refers to everything matched by the pattern |

---

## ⚠️ Important Points

* **`\L` and `\U` are replacement-side only** — they live inside `s/pattern/\L&/` replacement section, not in the search pattern. Writing `/\L/` as a search pattern does nothing useful
* **`[a-z]` checks content AFTER extraction** — the guard runs after `s/.*: //` strips the tag. So it correctly checks the content ("THIS IS LOW"), not the full line ("low : THIS IS LOW") which always has lowercase "low"
* **Order inside block matters** — `s/.*: //` must come before the guard `/[a-z]/!`. If reversed, the guard would check the full line including the tag and always find lowercase letters in `low`
* **`&` in `\L&`** — the `.*` matches the whole line, `&` recalls it, `\L` lowercases it. You could also write `s/.*/\L\0/` but `&` is the standard way

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Run commands only on tagged lines | `/^tag/ { }` | `/^low/ { ... }` |
| Check if lowercase exists | `/[a-z]/` | guards conversion |
| Check if uppercase exists | `/[A-Z]/` | guards conversion |
| Do something only if pattern ABSENT | `/pattern/!` | `/[a-z]/! convert` |
| Convert whole line to lowercase | `s/.*/\L&/` | all chars lowercased |
| Convert whole line to uppercase | `s/.*/\U&/` | all chars uppercased |
| Strip a label/prefix | `s/.*: //` | remove "low : " tag |
| Refer to entire match in replacement | `&` | `s/.*/prefix_&_suffix/` |

> 💡 **Golden Rule — Guard Before Convert:**
> ```
> /[a-z]/! s/.*/\L&/    ← only lowercase if NO lowercase present
> /[A-Z]/! s/.*/\U&/    ← only uppercase if NO uppercase present
> ```
> The `!` negation turns a "what exists" check into a "safe to convert" gate.
> Read it as: **"if this case is NOT already present, then apply conversion."**
