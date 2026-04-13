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
1~3  s/^/S1,/
1~3! s/^/S2,/
'
```

---

## 🔍 Explanation

### How it works — line by line trace

```
Line 1 → 1~3  matches → s/^/S1,/ → S1,Ajay,Chennai,Sales,40
Line 2 → 1~3! matches → s/^/S2,/ → S2,Mona,Mumbai,Sales,60
Line 3 → 1~3! matches → s/^/S2,/ → S2,Rajan,Patna,Buisness,50
Line 4 → 1~3  matches → s/^/S1,/ → S1,Narayanan,Kochi,Sales,30
Line 5 → 1~3! matches → s/^/S2,/ → S2,Anuj,Pune,Sales,70
Line 6 → 1~3! matches → s/^/S2,/ → S2,Mohan,Jaipur,Buisness,70
```

### 🔹 Step Selection — `1~3`

```bash
1~3
```

* `first~step` → start at line `first`, then pick every `step`-th line
* `1~3` matches lines: **1, 4, 7, 10 ...**
* These lines get the command `s/^/S1,/` applied

```
1~3  →  1, 4, 7, 10 ...   (S1 lines)
2~3  →  2, 5, 8, 11 ...
0~2  →  2, 4, 6, 8  ...   (every even line)
1~2  →  1, 3, 5, 7  ...   (every odd line)
```

---

### 🔹 Negation — `1~3!`

```bash
1~3!
```

* `!` flips the address — runs on every line that **does NOT** match `1~3`
* So it matches lines: **2, 3, 5, 6, 8, 9 ...**
* These lines get the command `s/^/S2,/` applied
* `1~3` and `1~3!` are **mutually exclusive** — every line falls into exactly one group, no line is skipped or double-processed

---

### 🔹 Prepend with `^` Anchor — `s/^/text/`

```bash
s/^/S1,/
```

* `^` matches the **start of the line** — it is a zero-width position, not a character
* Substituting `^` with `S1,` **inserts** the text before the first character
* Nothing is deleted — the original content is fully preserved

```
Before:  Ajay,Chennai,Sales,40
After:   S1,Ajay,Chennai,Sales,40
```

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Step Addressing | `first~step` | Selects line `first` and every `step`-th line after it |
| 2 | Negation | `address!` | Inverts the address — runs on lines that do NOT match |
| 3 | Substitution | `s/pattern/replacement/` | Finds pattern and replaces with replacement |
| 4 | Start-of-line Anchor | `^` | Zero-width match at the beginning of a line — used to prepend text |
| 5 | Default Print | *(no `-n`)* | sed prints every line automatically after processing |

---

## ⚠️ Important Points

* `s/^/text/` **prepends** — it never deletes anything, `^` is just a position
* No `-n` flag needed here — sed prints every line by default, which is exactly what we want
* `1~3` and `1~3!` together cover **all lines** — they act like an if/else
* The `~` character means **step**, the `,` character means **range** — never mix them up

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Every N-th line from a start | `first~step` | `1~3` → lines 1,4,7 |
| All lines NOT in a pattern | `address!` | `1~3!` → all except 1,4,7 |
| Add text at the START of a line | `s/^/text/` | `s/^/S1,/` |
| Add text at the END of a line | `s/$/text/` | `s/$/,END/` |
| Two mutually exclusive groups | `addr cmd` + `addr! cmd` | S1 vs S2 labeling |
| Every odd line | `1~2` | lines 1,3,5,7... |
| Every even line | `0~2` | lines 2,4,6,8... |
| Specific single line | `N` (just the number) | `5 s/^/X,/` |

> 💡 **Golden Rule:** If you need to split all lines into two groups and do different things to each — use `address` for group 1 and `address!` for group 2. They will always cover every line with zero overlap.
