# Extract Email Domain using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Reads input containing records with email addresses
* Extracts and prints only the **domain name** from each email
* The domain name is everything **after `@`**
* Ignores all other fields

---

## 📥 Sample Input

```text
Mehta,1-322-248-9102,mehta@yahoo.net,Daman and Diu
Malik,1-335-746-5592,malik@hotmail.com,Andhra Pradesh
Suri,1-134-516-1754,suri@protonmail.org,Lakshadweep
Persaud,268-1152,persaud302@aol.co.uk,Dadra and Nagar Haveli
Joshi,1-869-326-6582,joshi4288@hotmail.com,OR
Subram,1-172-433-4318,subram@hotmail.edu,MH
Sehgal,647-1701,sehgal@outlook.co.uk,DD
```

---

## 📤 Sample Output

```text
yahoo.net
hotmail.com
protonmail.org
aol.co.uk
hotmail.com
hotmail.edu
outlook.co.uk
```

---

## ✅ Solution — CLI

```bash
sed -E 's/.*@//; s/,.*//'
```

## ✅ Solution — script.sed

```sed
s/.*@//
s/,.*//
```

```bash
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — trace for `Mehta,1-322-248-9102,mehta@yahoo.net,Daman and Diu`

```
Input:   Mehta,1-322-248-9102,mehta@yahoo.net,Daman and Diu

Step 1 — s/.*@//
         .*  matches everything greedily up to the LAST @
         Mehta,1-322-248-9102,mehta@  ← deleted
         Remaining: yahoo.net,Daman and Diu

Step 2 — s/,.*//
         ,   matches the first comma after the domain
         ,Daman and Diu  ← deleted
         Remaining: yahoo.net  ✓
```

---

### 🔹 Step 1 — Remove Everything Up to and Including `@`

```bash
s/.*@//
```

* `.*` → matches **any characters, zero or more** — this is greedy, matches as much as possible
* `@` → matches the literal `@` symbol
* Together `.*@` matches everything from the start up to and including the `@`
* Replacement is empty `//` → everything matched is deleted
* What remains: `domain,rest-of-line`

```
Mehta,1-322-248-9102,mehta@yahoo.net,Daman and Diu
└────────────────────────────┘
          deleted by .*@

Remaining: yahoo.net,Daman and Diu
```

> **Why greedy matters here:** `.*@` greedily matches as far right as possible before `@`. Since there is only one `@` in the line, it correctly consumes everything before it.

---

### 🔹 Step 2 — Remove Everything From the Next Comma Onwards

```bash
s/,.*//
```

* `,` → matches the first comma after the domain
* `.*` → matches everything after that comma to end of line
* Replacement is empty `//` → the comma and everything after it is deleted
* What remains: just the domain

```
yahoo.net,Daman and Diu
         └─────────────┘
            deleted by ,.*

Remaining: yahoo.net  ✓
```

---

### 🔹 Why Two Steps and Not One?

You could try to do it in one substitution using a capture group:

```bash
sed -E 's/.*@([^,]+),.*/\1/'
```

Both approaches produce the same result. The two-step version is simpler to read:

| Approach | Syntax | Readability |
|---|---|---|
| Two substitutions | `s/.*@//; s/,.*//` | ✅ Easy — delete left, delete right |
| One substitution | `s/.*@([^,]+).*/\1/` | Harder — requires capture group |

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Greedy wildcard | `.*` | Matches zero or more of any character, as many as possible |
| 2 | Literal match | `@` and `,` | Matches these exact characters as anchors |
| 3 | Delete by replacing with empty | `s/pattern//` | Removes matched portion — replacement is empty string |
| 4 | Chained substitutions | `s/a//; s/b//` | Two `s` commands run sequentially on the same line |
| 5 | No `-n` needed | *(default print)* | sed prints every processed line automatically |

---

## ⚠️ Important Points

* **Order matters** — you must remove left of `@` first, then remove right of domain. Reversing the order breaks the logic because `.*@` needs the `@` to still be present
* `.*` is **greedy** — `s/.*@//` always removes everything up to the rightmost `@`. Since emails have exactly one `@`, this works perfectly
* No `-E` flag needed for `script.sed` — neither `.*` nor `@` nor `,` requires extended regex. The `-E` in the CLI version is optional here (included out of habit but makes no difference for this specific pattern)
* Works for **any domain format** — `.com`, `.org`, `.co.uk`, `.edu` — because the second substitution only looks for a comma, not a specific domain pattern

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Delete everything **before** a marker | `s/.*marker//` | `s/.*@//` removes up to `@` |
| Delete everything **after** a marker | `s/marker.*//` | `s/,.*//` removes from `,` onwards |
| Extract text **between** two markers | Two-step delete OR capture group | Delete left then right |
| Keep a specific field in CSV | `s/.*@//` then `s/,.*//` | Isolate the domain column |
| Reuse the extracted part elsewhere | `s/.*@([^,]+).*/\1/` with `-E` | Capture group + backreference |

> 💡 **Golden Rule — Two-Step Extraction:**
> To extract text that is **between two known delimiters**:
> 1. First `s/.*LEFT_DELIM//` → strip everything to the left
> 2. Then `s/RIGHT_DELIM.*//` → strip everything to the right
>
> What remains in the middle is your answer.
> This "left-strip then right-strip" pattern works for any field extraction in sed.
