# Employee Record Transformation using sed

---

## 📝 Problem Statement

Write a **sed script** (`employees.sed`) that:
* Converts `Developer` → `Senior Developer`
* Removes the age field (`, <digits>,`) from each record
* Script must be executable and run via `sed -f` with a heredoc

---

## 📥 Sample Input

```text
Rahul, 30, Developer
Emily, 25, Designer
Amit, 28, Manager
Sarah, 35, Developer
James, 32, Designer
```

---

## 📤 Sample Output

```text
Rahul, Senior Developer
Emily, Designer
Amit, Manager
Sarah, Senior Developer
James, Designer
```

---

## ❌ Given Solution — Two Bugs

```sed
s/Developer/Senior Developer/
s/[0-9]+, //           ← BUG 1: + is literal in BRE (needs -E or \+)
                       ← BUG 2: wrong pattern — removes "30, " but keeps leading ", "
```

**Actual output of the broken script:**
```
Rahul, 30, Senior Developer   ← age NOT removed (+ treated as literal)
Emily, 25, Designer
...
```

---

## ✅ Correct Solution — employees.sed (BRE)

```sed
s/Developer/Senior Developer/
s/, [0-9]\+,/,/
```

**Save and make executable:**
```bash
cat > employees.sed << 'EOF'
s/Developer/Senior Developer/
s/, [0-9]\+,/,/
EOF
chmod +x employees.sed
```

**Run with file:**
```bash
sed -f employees.sed input.txt
```

**Run with heredoc:**
```bash
sed -f employees.sed << 'EOF'
Rahul, 30, Developer
Emily, 25, Designer
Amit, 28, Manager
Sarah, 35, Developer
James, 32, Designer
EOF
```

---

## ✅ Correct Solution — employees.sed (ERE)

```sed
s/Developer/Senior Developer/
s/, [0-9]+,/,/
```

```bash
sed -E -f employees.sed input.txt
```

---

## 🔍 Explanation

### How it works — trace for `Rahul, 30, Developer`

```
Input:   Rahul, 30, Developer

Step 1 — s/Developer/Senior Developer/
  "Developer" found → replaced
  Result: Rahul, 30, Senior Developer

Step 2 — s/, [0-9]\+,/,/
  Pattern ", 30," found → replaced with ","
  Result: Rahul, Senior Developer  ✓
```

---

### 🔹 Bug 1 — `+` in BRE vs ERE

```bash
s/[0-9]+, //     ← BRE: + is a LITERAL plus sign, not "one or more"
s/[0-9]\+, //    ← BRE: \+ means "one or more"  ✓
s/[0-9]+, //     ← ERE (with -E): + means "one or more"  ✓
s/[0-9][0-9]*, // ← BRE: [0-9][0-9]* means "one or more digits"  ✓
```

| Pattern | Mode | Behaviour |
|---|---|---|
| `[0-9]+` | BRE (default) | matches literal `[0-9]+` — looks for a `+` character |
| `[0-9]\+` | BRE | matches one or more digits ✓ |
| `[0-9]+` | ERE (`-E`) | matches one or more digits ✓ |
| `[0-9][0-9]*` | BRE | matches one or more digits ✓ (no `+` needed) |

**Proof:**
```bash
echo "Rahul, 30, Developer" | sed 's/[0-9]+, /REMOVED/'
# Output: Rahul, 30, Developer  ← unchanged! + was literal, no match

echo "Rahul, 30, Developer" | sed 's/[0-9]\+, /REMOVED/'
# Output: Rahul, REMOVEDDeveloper  ← but structurally still wrong (Bug 2)
```

---

### 🔹 Bug 2 — Wrong Pattern Structure

The original pattern `s/[0-9]+, //` tries to match the age + the comma+space after it. But the structure of the input is:

```
Rahul, 30, Developer
      ↑  ↑
      |  comma+space between age and role
      comma+space between name and age
```

If you delete only `30, ` (digits + comma + space after):
```
Rahul, 30, Developer
       └──┘ deleted: "30, "
Result: Rahul, Developer   ← extra leading ", " left → "Rahul,  Developer" or wrong
```

The correct approach is to include the **surrounding commas** in the match:

```bash
s/, [0-9]\+,/,/
```

This matches `, 30,` (comma-space + digits + comma) and replaces with a single `,`:

```
Rahul, 30, Developer
      └───┘
   ", 30," → ","
Result: Rahul, Developer  ✓
```

**Visual proof:**
```
"Rahul, 30, Developer"
        ↑↑↑↑
   match: ", 30,"
replace:  ","
result: "Rahul, Developer"  ✓
```

---

### 🔹 Step 1 — Simple Substitution

```bash
s/Developer/Senior Developer/
```

* Literal string replacement — no regex needed
* Only lines containing `Developer` are affected
* `Designer`, `Manager` pass through unchanged

---

### 🔹 Step 2 — Remove Age Field

```bash
s/, [0-9]\+,/,/
```

* `, ` → literal comma + space (before the age)
* `[0-9]\+` → one or more digits (BRE syntax for `+`)
* `,` → literal comma (after the age)
* Replacement `,` → keeps the structural comma, only removes the age value

---

### 🔹 Heredoc Usage

A heredoc lets you pass multi-line input directly to a command without a file:

```bash
sed -f employees.sed << 'EOF'
Rahul, 30, Developer
Emily, 25, Designer
EOF
```

* `<< 'EOF'` → heredoc delimiter — everything between this and `EOF` is sent as stdin
* The single quotes around `'EOF'` prevent shell variable expansion inside the heredoc
* `sed -f employees.sed` reads the script file and processes the heredoc input

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Simple substitution | `s/old/new/` | Replaces literal string |
| 2 | BRE one-or-more | `\+` | Matches one or more of previous (BRE version of `+`) |
| 3 | Digit class | `[0-9]` | Matches any single digit |
| 4 | Structural pattern | `/, [0-9]\+,/` | Matches the whole age field including surrounding commas |
| 5 | sed script file | `-f script.sed` | Reads commands from file instead of inline |
| 6 | Heredoc | `<< 'EOF'` | Passes multi-line text as stdin to a command |
| 7 | chmod +x | `chmod +x script.sed` | Makes the script file executable |

---

## ⚠️ Important Points

* **`+` in BRE is literal** — the most common sed mistake. Always use `\+` in BRE or add `-E` for ERE when you need "one or more"
* **Match surrounding commas, not just digits** — `s/[0-9]\+, //` removes `30, ` but leaves a dangling `, ` before the role. Always include both delimiters: `s/, [0-9]\+,/,/`
* **Single quotes in `'EOF'`** — when your heredoc input contains `$` or other special chars, use `<< 'EOF'` to prevent shell expansion. Plain `<< EOF` would expand variables inside the heredoc body
* **Order of commands matters** — run `s/Developer/Senior Developer/` BEFORE `s/, [0-9]\+,/,/`. Reversing still works here, but the convention is logical order: first transform content, then clean structure

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| One-or-more in BRE (no `-E`) | `\+` | `[0-9]\+` |
| One-or-more in ERE | `+` with `-E` | `[0-9]+` |
| Remove a field AND its delimiters | Match both delimiters | `s/, age,/,/` |
| Remove only the value (keep structure) | Match surrounding separators | `s/, [0-9]\+,/,/` |
| Pass multi-line input without a file | heredoc `<< 'EOF'` | `sed -f s.sed << 'EOF'` |
| Prevent shell expansion in heredoc | Quote the delimiter `'EOF'` | `<< 'EOF'` not `<< EOF` |
| Simple word replacement | `s/word/new word/` | no regex needed |

> 💡 **Golden Rule — Removing a CSV Field:**
> To remove a field from a delimited record, always match
> **both the field value AND its surrounding delimiters** in the pattern,
> then replace with just one delimiter:
> ```
> Input:    Name, AGE, Role
> Pattern:  s/, [0-9]\+,/,/
>             ↑          ↑  ← both commas in pattern
> Replace:  ,             ← single comma keeps structure intact
> Output:   Name, Role    ✓
> ```
