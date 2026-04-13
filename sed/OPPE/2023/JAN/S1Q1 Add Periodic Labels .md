# Add Periodic Labels using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Adds a new column at the **beginning of each line**
* Inserts `"S1"` for every **3rd line starting from line 1** (1, 4, 7, ...)
* Inserts `"S2"` for all other lines

---

## 📥 Sample Input

```text
Ajay,Chennai,Sales,40
Mona,Mumbai,Sales,60
Rajan,Patna,Buisness,50
Narayanan,Kochi,Sales,30
Anuj,Pune,Sales,70
Mohan,Jaipur,Buisness,70
```

---

## 📤 Sample Output

```text
S1,Ajay,Chennai,Sales,40
S2,Mona,Mumbai,Sales,60
S2,Rajan,Patna,Buisness,50
S1,Narayanan,Kochi,Sales,30
S2,Anuj,Pune,Sales,70
S2,Mohan,Jaipur,Buisness,70
```

---

## 💻 Solution

```bash
sed '
1~3 s/^/S1,/
1~3! s/^/S2,/
'
```

---

## 🔍 Explanation

### 🔹 Step Selection

```bash
1~3
```

* Selects every 3rd line starting from line 1
* Matches lines: 1, 4, 7, 10, ...

---

### 🔹 Negation

```bash
1~3!
```

* Selects all lines **not matching** the above condition

---

### 🔹 Add Prefix

```bash
s/^/S1,/
```

* Adds `"S1,"` at the beginning of selected lines

```bash
s/^/S2,/
```

* Adds `"S2,"` for remaining lines

---

## Concept: Step Addressing (`first~step`) + Negation (`!`) + Anchor Substitution (`^`)

---

### 🧠 What Concepts Are Being Tested?

| Concept | Syntax Used | Purpose |
|---|---|---|
| Step addressing | `1~3` | Select line 1, then every 3rd line after (1, 4, 7...) |
| Negation | `1~3!` | Select ALL lines that are NOT in the step pattern |
| Substitution with `^` anchor | `s/^/prefix,/` | Insert text at the very beginning of a line |

---

### 🔵 Concept 1 — Step Addressing: `first~step`

**Syntax:** `first~step`

- `first` → the line number to start from
- `step` → select every `step`-th line after `first`

```
1~3  →  matches lines: 1, 4, 7, 10, 13 ...
2~3  →  matches lines: 2, 5, 8, 11, 14 ...
0~2  →  matches lines: 2, 4, 6, 8  (every even line)
1~2  →  matches lines: 1, 3, 5, 7  (every odd line)
```

**In this question:**
```bash
1~3 s/^/S1,/
# Runs on lines 1, 4, 7 → prepends "S1,"
```

---

### 🔵 Concept 2 — Negation: `address!command`

**Syntax:** `address!command`

The `!` inverts the address — the command runs on every line that does **NOT** match the address.

```bash
1~3! s/^/S2,/
# Runs on every line that is NOT 1, 4, 7...
# i.e., runs on lines 2, 3, 5, 6, 8, 9 → prepends "S2,"
```

**Key insight:** `1~3` and `1~3!` together are **exhaustive and mutually exclusive** — every line falls into exactly one of the two groups. No line is missed, no line is double-processed.

---

### 🔵 Concept 3 — `^` Anchor in Substitution

**Syntax:** `s/^/text/`

`^` matches the **start of the line** (zero-width — it matches a position, not a character). Replacing it with text effectively **prepends** that text to the line.

```
Input line:   Ajay,Chennai,Sales,40
s/^/S1,/
Output line:  S1,Ajay,Chennai,Sales,40
```

---

## ⚠️ Important Points

* `^` → start of line
* No need for `-n`, `p`, or `d`
* sed prints automatically

---

## 🧠 Memory Trick

👉 **1~3 → S1, rest → S2**
