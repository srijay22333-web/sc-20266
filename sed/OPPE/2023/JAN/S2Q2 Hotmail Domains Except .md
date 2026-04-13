# Filter Hotmail Domains (Except hotmail.com) using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Reads records containing email addresses
* Prints only the lines where the email domain contains **"hotmail"**
* Excludes lines where the domain is exactly **"hotmail.com"**

---

## 📥 Sample Input

```text
name,phone,email,region
Sahni,151-8534,sahni@hotmail.ca,KA
Mehta,1-322-248-9102,mehta@yahoo.net,Daman and Diu
Malik,1-335-746-5592,malik@hotmail.com,Andhra Pradesh
Suri,1-134-516-1754,suri@protonmail.org,Lakshadweep
Persaud,268-1152,persaud302@aol.co.uk,Dadra and Nagar Haveli
Joshi,1-869-326-6582,joshi4288@hotmail.com,OR
Subram,1-172-433-4318,subram@hotmail.edu,MH
```

---

## 📤 Sample Output

```text
Sahni,151-8534,sahni@hotmail.ca,KA
Subram,1-172-433-4318,subram@hotmail.edu,MH
```

---

## ✅ Solution — CLI

```bash
sed '/hotmail/ {/hotmail\.com/d}; /hotmail/!d'
```

## ✅ Solution — script.sed

```sed
/hotmail/ {
/hotmail\.com/d
}
/hotmail/!d
```

```bash
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — full trace for every input line

```
Line: name,phone,email,region
      /hotmail/ → NO match → block skipped
      /hotmail/!d → NOT hotmail → DELETED ✗

Line: Sahni,151-8534,sahni@hotmail.ca,KA
      /hotmail/ → MATCH → enter block
        /hotmail\.com/d → "hotmail.ca" ≠ "hotmail.com" → NOT deleted
      /hotmail/!d → IS hotmail → NOT deleted
      → PRINTED ✓

Line: Mehta,1-322-248-9102,mehta@yahoo.net,Daman and Diu
      /hotmail/ → NO match → block skipped
      /hotmail/!d → NOT hotmail → DELETED ✗

Line: Malik,1-335-746-5592,malik@hotmail.com,Andhra Pradesh
      /hotmail/ → MATCH → enter block
        /hotmail\.com/d → "hotmail.com" MATCHES → DELETED ✗

Line: Joshi,1-869-326-6582,joshi4288@hotmail.com,OR
      /hotmail/ → MATCH → enter block
        /hotmail\.com/d → "hotmail.com" MATCHES → DELETED ✗

Line: Subram,1-172-433-4318,subram@hotmail.edu,MH
      /hotmail/ → MATCH → enter block
        /hotmail\.com/d → "hotmail.edu" ≠ "hotmail.com" → NOT deleted
      /hotmail/!d → IS hotmail → NOT deleted
      → PRINTED ✓
```

---

### 🔹 Pattern Match Block — `/pattern/ { commands }`

```bash
/hotmail/ {
  /hotmail\.com/d
}
```

* `/hotmail/` → address that selects lines containing `"hotmail"`
* `{ }` → groups commands to run **only on the matched lines**
* `/hotmail\.com/d` → inside the block, delete if it also matches `hotmail.com`
* Lines with `hotmail.ca`, `hotmail.edu` etc. enter the block but survive because they don't match `/hotmail\.com/`
* Lines without `hotmail` never enter the block at all

This is **nested addressing** — an inner condition inside an outer condition.

---

### 🔹 Escaped Dot — `\.`

```bash
/hotmail\.com/
```

* In regex, `.` means **any character** — `hotmail.com` without escaping would also match `hotmailXcom`
* `\.` escapes the dot → matches only a **literal period**
* This ensures only exact `hotmail.com` is deleted, not accidental matches like `hotmail_com`

```
hotmail.com   →  matches /hotmail\.com/  → DELETED
hotmail.ca    →  does NOT match          → KEPT
hotmailXcom   →  matches /hotmail.com/   → would be deleted (if . not escaped!)
hotmailXcom   →  does NOT match /hotmail\.com/ → KEPT (with escaped dot)
```

---

### 🔹 Delete Non-Matching Lines — `/hotmail/!d`

```bash
/hotmail/!d
```

* `/hotmail/` → lines containing `hotmail`
* `!` → negation — inverts the address
* `d` → delete
* Combined: **delete every line that does NOT contain `hotmail`**
* This removes the header line, yahoo, protonmail, aol lines etc.

---

### 🔹 Why Both Commands Are Needed

| Command | What it removes |
|---|---|
| `/hotmail/ { /hotmail\.com/d }` | Lines that have `hotmail` AND are `hotmail.com` specifically |
| `/hotmail/!d` | Lines that have NO `hotmail` at all |

```
All lines
├── contains "hotmail"
│   ├── is "hotmail.com"  → deleted by inner block
│   └── is other hotmail  → KEPT ✓
└── does NOT contain "hotmail"  → deleted by /hotmail/!d
```

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Regex address | `/pattern/` | Selects lines matching the pattern |
| 2 | Command block | `{ }` | Groups commands under one address |
| 3 | Nested addressing | `/p1/ { /p2/cmd }` | Runs inner command only when both conditions are true |
| 4 | Negation | `!` | Inverts the address — runs on non-matching lines |
| 5 | Delete | `d` | Removes current line from output |
| 6 | Escaped dot | `\.` | Matches a literal `.` instead of any character |

---

## ⚠️ Important Points

* **`\.` is essential** — without escaping, `/hotmail.com/` would match `hotmailXcom` or any character in place of `.`
* **Order matters** — the block `{/hotmail\.com/d}` must come before `/hotmail/!d`. If reversed, the delete-non-hotmail runs first and the hotmail.com lines would already be gone before the block gets a chance
* The `d` command **immediately ends the cycle** — once a line is deleted, no further commands run on it
* **No `-n` needed** — we are deleting unwanted lines, not selectively printing. Surviving lines print automatically

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Run commands only on matching lines | `/pattern/ { }` | `/hotmail/ { ... }` |
| Match BOTH condition A and B | Nest `/B/cmd` inside `/A/ { }` | `/hotmail/ { /\.com/d }` |
| Keep only lines matching a pattern | `/pattern/!d` | `/hotmail/!d` |
| Match literal dot in regex | `\.` | `hotmail\.com` |
| Delete lines matching a pattern | `/pattern/d` | `/hotmail\.com/d` |
| Two-condition filter (AND logic) | Nested blocks | `/A/ { /B/d }` |
| Two-condition filter (OR logic) | `/A/!d` after handling exceptions | keep A, remove non-A |

> 💡 **Golden Rule — Nested Blocks for AND logic:**
> When you need **"lines that match A but NOT B"**:
> ```
> /A/ { /B/d }    ← delete lines that are both A and B
> /A/!d           ← delete lines that are not A at all
> ```
> What survives = lines that are A but not B — exactly what you want.
