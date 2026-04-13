# Mask Phone Numbers using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Masks the **first 6 digits** of a 10-digit phone number
* Replaces them with `******`
* Keeps only the **last 4 digits visible**

---

## 📥 Sample Input

```text
9876543210
1234567890
5556667777
```

---

## 📤 Sample Output

```text
******3210
******7890
******7777
```

---

## 💻 Solution 1 — Using Capture Group

```bash
sed -E 's/^[0-9]{6}([0-9]{4})/******\1/'
```

## 💻 Solution 2 — Simple Replacement

```bash
sed -E 's/^[0-9]{6}/******/'
```

---

## 🔍 Explanation

### How it works — trace for `9876543210`

```
Input:    9876543210
          ^^^^^^^^^^
          123456 7890   ← position breakdown

s/^[0-9]{6}([0-9]{4})/******\1/

^           → anchor at start of line
[0-9]{6}    → matches "987654"  → replaced with "******"
([0-9]{4})  → captures "3210"   → recalled as \1
Output:   ******3210
```

---

### 🔹 Solution 1 — Match + Capture + Backreference

```bash
^[0-9]{6}([0-9]{4})
```

* `^` → anchor — match must start at the beginning of the line
* `[0-9]{6}` → matches exactly 6 digits — these are the ones being masked
* `([0-9]{4})` → captures the next 4 digits into group `\1`

```bash
******\1
```

* `******` → replaces the first 6 digits
* `\1` → puts back the captured 4 digits exactly as they were

```
9876543210
├──────┤└──┘
 masked  \1
******  3210  →  ******3210
```

---

### 🔹 Solution 2 — Match and Replace Only the First Part

```bash
^[0-9]{6}
```

* Matches only the first 6 digits from the start of the line
* The remaining 4 digits are **never touched** — sed leaves them as-is
* No capture group needed because we are not rearranging anything

```
9876543210
├──────┤└──┘
 matched  untouched
******    3210  →  ******3210
```

> Both solutions produce identical output. Solution 2 is simpler.
> Solution 1 is useful when you need to **rearrange** parts of the match.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Extended Regex flag | `-E` | Enables `{n}` and `(...)` without backslashes |
| 2 | Start-of-line anchor | `^` | Ensures match begins at position 0 of the line |
| 3 | Character class | `[0-9]` | Matches any single digit 0 through 9 |
| 4 | Exact quantifier | `{6}` / `{4}` | Matches exactly N occurrences of the preceding pattern |
| 5 | Capture group | `([0-9]{4})` | Saves matched text so it can be reused in replacement |
| 6 | Backreference | `\1` | Recalls the text saved by capture group 1 |

---

## ⚠️ Important Points

* `-E` is **required** — without it, `{6}` is treated as literal characters, not a quantifier
* No `g` flag needed — there is only one phone number per line and `^` anchors to start, so only one match is possible
* Solution 1 matches all 10 digits but reconstructs the last 4 via `\1` — the whole line is "replaced"
* Solution 2 matches only 6 digits — the last 4 are never part of the match so they survive untouched
* Both only work correctly for **exactly 10-digit** numbers — a different length would break the fixed `{6}` + `{4}` assumption

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Match exactly N digits | `[0-9]{N}` with `-E` | `[0-9]{6}` → 6 digits |
| Anchor match to line start | `^` | `^[0-9]{6}` → only at beginning |
| Keep part of the match in output | Capture group `(...)` + `\1` | `([0-9]{4})` → recall as `\1` |
| Discard a prefix, keep the rest | Just match the prefix, replace it | `s/^[0-9]{6}/******/` |
| Rearrange parts of a match | Capture both parts, swap in replacement | `s/(A)(B)/\2\1/` |
| Use `{}` quantifiers cleanly | Always use `-E` | `sed -E 's/[0-9]{6}//'` |

> 💡 **Golden Rule — Solution 1 vs Solution 2:**
> * If you only need to **replace a prefix** → match just that prefix, no capture needed (Solution 2)
> * If you need to **keep and reuse part of what you matched** → capture it with `(...)` and recall with `\1` (Solution 1)
> * When in doubt, Solution 2 is always simpler for masking/trimming from one end
