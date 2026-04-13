# Mask Phone Numbers in Text using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Masks the **first 6 digits** of a 10-digit phone number
* Keeps only the **last 4 digits visible**
* The phone number:
  * Can appear **anywhere in the text** (not just at line start)
  * Must be exactly **10 digits**
  * Must start with **6, 7, 8, or 9**
* Other numbers (reference IDs, codes) remain **unchanged**

---

## 📥 Sample Input

```text
Yesterday I received a message from 9876543210, it has numerical sequence
1234567890 in it. then soon after I received the same message from 9876543211
as well. Then I called 9876543212 to inquire about it, they gave a reference
id AHDISJ12354 and a fallback code 3245112341 as well.
```

---

## 📤 Sample Output

```text
Yesterday I received a message from ******3210, it has numerical sequence
1234567890 in it. then soon after I received the same message from ******3211
as well. Then I called ******3212 to inquire about it, they gave a reference
id AHDISJ12354 and a fallback code 3245112341 as well.
```

---

## ✅ Solution — CLI

```bash
sed -E 's/\b[6789][0-9]{5}([0-9]{4})\b/******\1/g'
```

## ✅ Solution — script.sed

```sed
s/\b[6789][0-9]{5}([0-9]{4})\b/******\1/g
```

```bash
sed -E -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — anatomy of the pattern

```
\b  [6789]  [0-9]{5}  ([0-9]{4})  \b
│     │         │           │       │
│     │         │           │       └── word boundary: number ends here
│     │         │           └────────── capture group \1: last 4 digits
│     │         └────────────────────── next 5 digits (positions 2–6)
│     └──────────────────────────────── first digit: must be 6,7,8 or 9
└────────────────────────────────────── word boundary: number starts here

Total digits matched = 1 + 5 + 4 = 10  ✓
Digits masked        = 1 + 5     = 6   → replaced with ******
Digits kept          =         4       → recalled as \1
```

---

### Trace — all numbers in the input

```
9876543210   → starts with 9 ✓, 10 digits ✓  → ******3210  (masked)
1234567890   → starts with 1 ✗               → unchanged
9876543211   → starts with 9 ✓, 10 digits ✓  → ******3211  (masked)
9876543212   → starts with 9 ✓, 10 digits ✓  → ******3212  (masked)
AHDISJ12354  → has letters, \b prevents match → unchanged
3245112341   → starts with 3 ✗               → unchanged
```

---

### 🔹 Word Boundary — `\b`

```bash
\b[6789][0-9]{5}([0-9]{4})\b
```

* `\b` marks the **boundary between a word character and a non-word character**
* Word characters = `[a-zA-Z0-9_]`, non-word = space, comma, punctuation
* Two `\b` anchors — one at the start, one at the end — ensure the pattern matches a **standalone number** only

```
"9876543210"  in "from 9876543210,"
               ↑                 ↑
          \b here (space→digit)  \b here (digit→comma)  ✓ matches

"AHDISJ12354"
        ↑
   no \b here between J and 1 (both are word chars)  ✗ no match
```

> Without `\b`, a number like `19876543210` (11 digits) could partially match, masking digits from the middle of a larger number.

---

### 🔹 First Digit Constraint — `[6789]`

```bash
[6789]
```

* A **character class** that matches exactly one character: `6`, `7`, `8`, or `9`
* Indian mobile numbers always start with these digits
* This excludes `1234567890` (starts with `1`) and `3245112341` (starts with `3`)

```
[6789]  →  matches: 6, 7, 8, 9
        →  rejects: 0, 1, 2, 3, 4, 5
```

---

### 🔹 Exact Quantifier — `{5}` and `{4}`

```bash
[0-9]{5}   →  matches exactly 5 digits
[0-9]{4}   →  matches exactly 4 digits (captured)
```

* `{N}` requires exactly N occurrences — available only with `-E` (ERE)
* `[6789]` + `[0-9]{5}` + `[0-9]{4}` = **1 + 5 + 4 = exactly 10 digits**
* In BRE (without `-E`) you would write `[0-9]\{5\}` — more verbose

---

### 🔹 Capture Group + Backreference

```bash
([0-9]{4})   →  captures last 4 digits as \1
```

Replacement: `******\1`

* `******` → 6 asterisks replacing the first 6 matched digits
* `\1` → recalls exactly what was captured — the last 4 digits

```
9876543210
├──────┤└──┘
[6789][0-9]{5}  ([0-9]{4})
  masked          \1 = 3210

Result: ******3210
```

---

### 🔹 Global Flag — `g`

```bash
s/.../...../g
```

* Without `g`: only the **first** phone number on each line is masked
* With `g`: **all** phone numbers on the line are masked
* The input has 3 valid numbers on one long line — `g` ensures all three are caught

---

### 🔹 This vs Previous Phone Masking Question

| Feature | Q: Mask Phone (simple) | Q: Mask Phone in Text |
|---|---|---|
| Number position | Always at line start | Anywhere in text |
| Anchor used | `^` (start of line) | `\b` (word boundary) |
| First digit check | None — any 10 digits | Must be `[6789]` |
| Other numbers | None present | Must be left unchanged |
| Flag needed | None | `g` (multiple per line) |

> When the number is the **only content on the line** → use `^`
> When the number is **embedded in text** → use `\b`

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | ERE flag | `-E` | Enables `{}`, `()`, without backslashes |
| 2 | Word boundary | `\b` | Ensures match is a standalone number, not part of a larger word or number |
| 3 | Character class | `[6789]` | Matches exactly one of the listed characters |
| 4 | Digit class | `[0-9]` | Matches any single digit 0–9 |
| 5 | Exact quantifier | `{5}`, `{4}` | Matches exactly N occurrences |
| 6 | Capture group | `([0-9]{4})` | Saves last 4 digits for reuse |
| 7 | Backreference | `\1` | Recalls the captured group in replacement |
| 8 | Global flag | `g` | Replaces all matches on the line, not just the first |

---

## ⚠️ Important Points

* **`\b` is critical here** — without it, numbers embedded in alphanumeric IDs like `AHDISJ12354` or longer digit strings could partially match
* **`[6789]` is part of the 6-digit mask** — the full mask is `[6789]` (1 digit) + `[0-9]{5}` (5 digits) = 6 digits total, replaced by `******`
* **`g` flag is mandatory** — the input has multiple phone numbers on one line; without `g` only the first would be masked
* **`-E` is required** for `{5}`, `{4}`, and `(...)` without backslashes — the `script.sed` file also needs `sed -E -f`

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Number is the whole line | `^` anchor | `s/^[0-9]{6}/******/` |
| Number is embedded in text | `\b` word boundary | `\b[6789][0-9]{9}\b` |
| First digit must be specific | `[6789]` class | restricts to Indian mobile range |
| Multiple matches per line | `g` flag | `s/pattern/replace/g` |
| Keep part of what you match | Capture group + `\1` | `([0-9]{4})` → `\1` |
| Use `{}` cleanly | `-E` flag | `sed -E 's/[0-9]{10}//'` |
| Avoid matching inside longer numbers | `\b` on both ends | `\b[0-9]{10}\b` |

> 💡 **Golden Rule — `^` vs `\b`:**
> * Use `^` when the number **starts at the beginning of the line** and nothing else is there
> * Use `\b` when the number is **surrounded by spaces, punctuation, or other text**
> * Using `^` for embedded numbers will silently fail — no error, just no match
