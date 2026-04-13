# Swap 4th Column to First using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Swaps the **fourth column** with the **first column**
* Keeps columns 2 and 3 in the same order

---

## 📥 Sample Input

```text
grep,219,10.95,47961
ls,208,10.4,43264
echo,195,9.75,38025
ps,92,4.6,8464
sed,82,4.1,6724
cd,80,4,6400
vi,77,3.85,5929
clear,72,3.6,5184
awk,72,3.6,5184
man,67,3.35,4489
```

---

## 📤 Sample Output

```text
47961,219,10.95,grep
43264,208,10.4,ls
38025,195,9.75,echo
8464,92,4.6,ps
6724,82,4.1,sed
6400,80,4,cd
5929,77,3.85,vi
5184,72,3.6,clear
5184,72,3.6,awk
4489,67,3.35,man
```

---

## ✅ Solution — CLI

```bash
sed -E 's/^([^,]+),([^,]+),([^,]+),([^,]+)/\4,\2,\3,\1/' mydata.csv
```

## ✅ Solution — script.sed

```sed
s/^([^,]+),([^,]+),([^,]+),([^,]+)/\4,\2,\3,\1/
```

```bash
sed -E -f script.sed mydata.csv
```

---

## 🔍 Explanation

### How it works — trace for `grep,219,10.95,47961`

```
Pattern:  ^([^,]+) , ([^,]+) , ([^,]+) , ([^,]+)
           └──┬──┘   └──┬──┘   └──┬──┘   └──┬──┘
             \1         \2         \3         \4

Matching:   grep    , 219     , 10.95   , 47961
             \1        \2        \3        \4

Replacement: \4  ,  \2  ,  \3  ,  \1
             47961 , 219 , 10.95 , grep

Output: 47961,219,10.95,grep  ✓
```

---

### 🔹 Start Anchor — `^`

```bash
^([^,]+)
```

* `^` anchors the match to the **beginning of the line**
* Ensures the pattern starts capturing from the very first character
* Without `^`, the regex could match anywhere in the line

---

### 🔹 Column Capture — `([^,]+)`

```bash
([^,]+)
```

This is the core building block — used **four times**, once per column.

* `[^,]` → a **negated character class** — matches any character **except** a comma
* `+` → one or more of those characters
* `( )` → wraps it in a **capture group** to save the match for reuse

```
[^,]+  on "grep,219,10.95,47961"
        └──┘
     matches "grep" — stops at the first comma
```

Why `[^,]+` and not `.*`?

```
[^,]+  →  stops at comma boundary  →  captures exactly one field  ✓
.*     →  greedy, crosses commas   →  may consume multiple fields  ✗
```

| Pattern | On `grep,219` | Result |
|---|---|---|
| `([^,]+),([^,]+)` | group1=`grep`, group2=`219` | ✅ Correct |
| `(.*),(.*)` | group1=`grep`, group2=`219` | ✅ Works here by backtracking |
| `(.*),(.*)` | on `a,b,c` | ❌ group1=`a,b`, group2=`c` (unpredictable) |

> Always use `[^,]+` for CSV column matching — it is **explicit and safe**.
> `.*` works by coincidence in simple cases but breaks on complex data.

---

### 🔹 Four Capture Groups

```bash
^([^,]+),([^,]+),([^,]+),([^,]+)
  ──\1──   ──\2──   ──\3──   ──\4──
```

* Each `([^,]+)` captures one CSV column
* The commas between groups are **literal separators** — they match the actual commas in the data but are NOT captured (not inside `()`)
* After matching: `\1`=col1, `\2`=col2, `\3`=col3, `\4`=col4

---

### 🔹 Rearrange with Backreferences

```bash
\4,\2,\3,\1
```

* Backreferences recall the captured groups in any order
* Here: put `\4` first, keep `\2` and `\3` in place, put `\1` last
* The commas in the replacement are **literal** — they rebuild the CSV structure

```
Original order:  \1  \2  \3  \4
                grep,219,10.95,47961

New order:       \4  \2  \3  \1
               47961,219,10.95,grep
```

---

### 🔹 Why `-E` is Required

| Feature | BRE (default) | ERE (`-E`) |
|---|---|---|
| Grouping | `\(` and `\)` | `(` and `)` |
| One or more | `\+` | `+` |
| BRE version of solution | `s/^\([^,]*\),\([^,]*\),\([^,]*\),\([^,]*\)/\4,\2,\3,\1/` | `s/^([^,]+),([^,]+),([^,]+),([^,]+)/\4,\2,\3,\1/` |

`-E` makes the pattern dramatically more readable — 4 groups in ERE vs 4 `\(` `\)` pairs in BRE.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | ERE flag | `-E` | Enables `()` and `+` without backslashes |
| 2 | Start anchor | `^` | Locks match to beginning of line |
| 3 | Negated character class | `[^,]` | Matches any character except comma |
| 4 | One-or-more quantifier | `+` | Requires at least one character per field |
| 5 | Capture group | `(...)` | Saves matched column for reuse in replacement |
| 6 | Backreference | `\1` `\2` `\3` `\4` | Recalls captured groups in any order |
| 7 | Column reordering | `\4,\2,\3,\1` | Rebuilds the line with columns in new order |

---

## ⚠️ Important Points

* **`[^,]+` not `.*`** — `[^,]+` stops at the comma boundary, making each group capture exactly one field. Using `.*` is greedy and unreliable for multi-field data
* **Commas between groups are not captured** — the `,` between `([^,]+)` and `([^,]+)` in the pattern matches the real comma but is consumed and discarded. You must manually add commas back in the replacement `\4,\2,\3,\1`
* **`-E` is required** — without it, `(` is a literal character, not a group. The entire pattern fails silently (no match = no substitution = original line printed unchanged)
* **No `g` flag needed** — there is only one set of 4 columns per line. The pattern matches the whole line once

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Capture a CSV column | `([^,]+)` | one group per column |
| Rearrange captured columns | backreferences in new order | `\4,\2,\3,\1` |
| Swap only col 1 and col 4 | `\4,\2,\3,\1` | keep middle unchanged |
| Swap only col 1 and col 2 | `\2,\1,\3,\4` | swap first two |
| Reverse all 4 columns | `\4,\3,\2,\1` | full reversal |
| Use groups cleanly | always use `-E` | avoids `\(` `\)` noise |
| Match field in any delimiter | `[^DELIM]+` | `[^:]+` for colon-separated |

> 💡 **Golden Rule — CSV Column Manipulation:**
> ```
> 1. Anchor with ^
> 2. Capture each column as ([^,]+)
> 3. Put literal commas between capture groups
> 4. In replacement, recall groups in the new order
> 5. Put literal commas between backreferences
> ```
> The number of commas in the pattern = number of commas in the replacement = number of columns minus one.
