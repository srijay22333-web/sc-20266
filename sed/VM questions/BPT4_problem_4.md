# Replace Second Occurrence of "error" using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Processes input line by line
* Replaces the **second occurrence** of the word `"error"` with `"issue"`
* Applies the replacement **only if the line starts with a hexadecimal code**
* Matching must be **case-insensitive**

---

## 📥 Sample Input

```text
1a2b3c4d This is an error message with an error that needs fixing.
5f6e7d8c No errors here, just a warning.
abcd1234 Another error followed by error.
1234abcd error error error error
nohexcode error should not be replaced here. or it will be an error.
```

---

## 📤 Sample Output

```text
1a2b3c4d This is an error message with an issue that needs fixing.
5f6e7d8c No errors here, just a warning.
abcd1234 Another error followed by issue.
1234abcd error issue error error
nohexcode error should not be replaced here. or it will be an error.
```

---

## ✅ Solution — CLI

```bash
sed '
/^[0-9A-Fa-f]/{
    s/error/issue/2I
}
' input.txt
```

---

## ✅ Solution — script.sed

```sed
/^[0-9A-Fa-f]/{
    s/error/issue/2I
}
```

```bash
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — step-by-step logic

```
Step 1: Check if line starts with hex
Step 2: Only then apply substitution
Step 3: Replace only 2nd occurrence of "error"
Step 4: Keep rest of line unchanged
```

---

### 🔹 Match Hexadecimal Lines — `/^[0-9A-Fa-f]/`

```bash
/^[0-9A-Fa-f]/
```

* `^` → start of line
* `[0-9A-Fa-f]` → any hexadecimal character
* Ensures only valid lines are processed

```
"1a2b..." → matches → processed
"nohex..." → does not match → skipped
```

---

### 🔹 Replace Second Occurrence — `s/error/issue/2I`

```bash
s/error/issue/2I
```

* `s///` → substitution command
* `2` → replaces only the **second occurrence**
* `I` → case-insensitive match

```
error error error
  1st   2nd   3rd

→ error issue error
```

---

## 🧩 Concepts Used in This Solution

| # | Concept            | Syntax         | What It Does                           |
| - | ------------------ | -------------- | -------------------------------------- |
| 1 | Addressing         | `/pattern/`    | Applies command only to matching lines |
| 2 | Hex match          | `^[0-9A-Fa-f]` | Ensures valid starting pattern         |
| 3 | Substitution       | `s///`         | Replaces text                          |
| 4 | Occurrence control | `/2`           | Replaces only 2nd match                |
| 5 | Case-insensitive   | `I`            | Matches `error`, `Error`, `ERROR`      |

---

## ⚠️ Important Points

* `2` replaces only the **second occurrence**, not all
* `I` is required for **case-insensitive matching**
* Non-hex lines are **not modified**
* Order: address → substitution

---

## 🧠 Memory — When to Use This Pattern

| Situation                   | Use This                  |
| --------------------------- | ------------------------- |
| Replace nth occurrence      | `s/pattern/replacement/n` |
| Case-insensitive replace    | add `I`                   |
| Apply only to certain lines | `/pattern/{}`             |

---

## 🧠 Memory Trick

👉 **Hex line → 2nd error → issue**

---

If you want next: I can give you a **“nth occurrence patterns cheat sheet (top 5 exam tricks)”**—this question is part of a bigger pattern 👍

