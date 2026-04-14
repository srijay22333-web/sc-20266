# Log File Transformation using sed 

---

## 📝 Problem Statement

Write a **sed script** that:
* Deletes `[DEBUG]` and `[INFO]` log lines entirely
* Deletes blank lines
* Converts `[WARNING]` → `[warning]`
* Converts `[ERROR]` → `[error]`
* Rewrites timestamps from `YYYY-MM-DD HH:MM:SS` → `HH:MM:SS DD-MM-YYYY`

---

## 📥 Sample Input

```text
[DEBUG] (2023-03-15 12:00:00) this is a debug message

[INFO] (2023-03-15 12:01:00) this is an info message

[WARNING] (2023-03-15 12:02:00) this is a warning message

[ERROR] (2023-03-15 12:03:00) this is an error message

[CRITICAL] (2023-03-15 12:04:00) this is a critical message
```

---

## 📤 Sample Output

```text
[warning] (12:02:00 15-03-2023) this is a warning message
[error] (12:03:00 15-03-2023) this is an error message
[CRITICAL] (12:04:00 15-03-2023) this is a critical message
```

---

## ✅ Solution — CLI

```bash
sed -E '
/\[DEBUG\]/d
/\[INFO\]/d
/^$/d
s/\[WARNING\]/[warning]/
s/\[ERROR\]/[error]/
s/\(([0-9]{4})-([0-9]{2})-([0-9]{2}) ([0-9]{2}:[0-9]{2}:[0-9]{2})\)/(\4 \3-\2-\1)/
' input.txt
```

## ✅ Solution — script.sed

```sed
/\[DEBUG\]/d
/\[INFO\]/d
/^$/d
s/\[WARNING\]/[warning]/
s/\[ERROR\]/[error]/
s/\(([0-9]{4})-([0-9]{2})-([0-9]{2}) ([0-9]{2}:[0-9]{2}:[0-9]{2})\)/(\4 \3-\2-\1)/
```

```bash
sed -E -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — step-by-step pipeline trace

```
After Step 1 (delete DEBUG, INFO, blanks):
  [WARNING] (2023-03-15 12:02:00) this is a warning message
  [ERROR]   (2023-03-15 12:03:00) this is an error message
  [CRITICAL](2023-03-15 12:04:00) this is a critical message

After Step 2 (case conversion):
  [warning] (2023-03-15 12:02:00) this is a warning message
  [error]   (2023-03-15 12:03:00) this is an error message
  [CRITICAL](2023-03-15 12:04:00) this is a critical message

After Step 3 (timestamp rewrite):
  [warning] (12:02:00 15-03-2023) this is a warning message
  [error]   (12:03:00 15-03-2023) this is an error message
  [CRITICAL](12:04:00 15-03-2023) this is a critical message  ✓
```

---

### 🔹 Delete Log Lines — `/\[DEBUG\]/d`

```bash
/\[DEBUG\]/d
/\[INFO\]/d
/^$/d
```

* `/\[DEBUG\]/` → matches lines containing literal `[DEBUG]`
* `\[` and `\]` → escaped brackets — in regex `[` starts a character class, so `\[` matches a **literal** `[`
* `d` → deletes the line immediately, no further commands run on it
* `/^$/d` → matches empty lines (`^` = start, `$` = end, nothing between = empty) and deletes them

```
"[DEBUG] ..." →  \[DEBUG\] matches  →  DELETED
"[INFO]  ..." →  \[INFO\]  matches  →  DELETED
""            →  ^$ matches         →  DELETED
"[WARNING]"   →  none match         →  KEPT
```

> `\[` is one of the most common escapes in sed.
> Without it, `[DEBUG]` would be interpreted as a character class
> matching `D`, `E`, `B`, `U`, `G` — not the literal string `[DEBUG]`.

---

### 🔹 Case Conversion — `s/\[WARNING\]/[warning]/`

```bash
s/\[WARNING\]/[warning]/
s/\[ERROR\]/[error]/
```

* Search: `\[WARNING\]` → literal string `[WARNING]` (brackets escaped in pattern)
* Replace: `[warning]` → literal string with lowercase (no escaping needed in replacement)
* Only the tag is changed — the rest of the line is untouched

```
"[WARNING] (2023...) message"
 └───────┘
   replaced
"[warning] (2023...) message"
```

> Note the asymmetry: in the **search** side `\[` escapes the bracket,
> but in the **replacement** side `[` is just a literal character.
> Regex rules apply only to the search pattern, not the replacement string.

---

### 🔹 Timestamp Rewrite — Anatomy of the Pattern

```bash
s/\(([0-9]{4})-([0-9]{2})-([0-9]{2}) ([0-9]{2}:[0-9]{2}:[0-9]{2})\)/(\4 \3-\2-\1)/
```

Breaking it down character by character:

```
\(                   → literal ( in the log line
  ([0-9]{4})         → group \1 = YYYY  (4 digits)
  -                  → literal hyphen
  ([0-9]{2})         → group \2 = MM    (2 digits)
  -
  ([0-9]{2})         → group \3 = DD    (2 digits)
  (space)            → literal space between date and time
  ([0-9]{2}          → group \4 = HH:MM:SS
    :[0-9]{2}
    :[0-9]{2})
\)                   → literal ) in the log line
```

**Capture group assignments:**

```
(2023-03-15 12:02:00)
  ^^^ ^^ ^^ ^^^^^^^^
 \1   \2  \3   \4
 YYYY  MM  DD  HH:MM:SS
```

**Replacement:** `(\4 \3-\2-\1)`

```
\4   = 12:02:00
\3   = 15
\2   = 03
\1   = 2023

Result: (12:02:00 15-03-2023)  ✓
```

---

### 🔹 Key Escaping Rules in This Pattern

| Character | In Search Pattern | In Replacement |
|---|---|---|
| `[` `]` (log tag) | `\[` `\]` — must escape | `[` `]` — no escape needed |
| `(` `)` (timestamp literal) | `\(` `\)` — must escape | `(` `)` — no escape needed |
| `(` `)` (capture group) | `(` `)` with `-E` | `\1` `\2` etc. to recall |
| `-` (hyphen in timestamp) | `-` — literal, no escape | `-` — literal |

> With `-E` (ERE), `(` means a **capture group** and `\(` means a **literal parenthesis**.
> This is why the outer timestamp parentheses use `\(` `\)` while
> the capture groups use plain `(` `)`.

---

### 🔹 Why `-E` Is Required

The timestamp pattern uses:
* `{4}` and `{2}` — exact quantifiers (ERE only, BRE needs `\{4\}`)
* `(...)` — capture groups (ERE uses plain `()`, BRE needs `\(\)`)

BRE equivalent (much harder to read):
```bash
s/(\([0-9]\{4\}\)-\([0-9]\{2\}\)-\([0-9]\{2\}\) \([0-9]\{2\}:[0-9]\{2\}:[0-9]\{2\}\))/(\4 \3-\2-\1)/
```

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Escaped brackets | `\[` `\]` | Matches literal `[` and `]` in log tags |
| 2 | Delete command | `/pattern/d` | Removes matching lines entirely |
| 3 | Empty line match | `/^$/d` | Deletes lines with nothing between start and end |
| 4 | Literal substitution | `s/\[WARNING\]/[warning]/` | Simple string replacement |
| 5 | ERE flag | `-E` | Enables `{n}` and `(...)` without backslashes |
| 6 | Escaped literal parens | `\(` `\)` | Matches literal `(` `)` inside ERE pattern |
| 7 | Capture groups | `([0-9]{4})` | Saves date/time parts for reordering |
| 8 | Backreferences | `\1` `\2` `\3` `\4` | Recalls captured groups in new order |
| 9 | Exact quantifier | `{4}` `{2}` | Matches exactly N digits |

---

## ⚠️ Important Points

* **`\[` in search vs `[` in replacement** — regex rules apply only to the search side. In replacement, `[warning]` is a plain literal string — no escaping needed
* **`\(` for literal vs `(` for group** — with `-E`, parentheses are groups by default. The timestamp is wrapped in real `(` `)` in the log file, so those must be `\(` `\)` in the pattern. The capture groups inside use plain `(` `)`
* **`d` ends the cycle immediately** — once a line matches `[DEBUG]` and `d` fires, no further commands run on that line. This means the order of `d` commands vs `s` commands does not matter for deleted lines — they never reach the substitution commands
* **`/^$/d` is needed** — the blank lines between log entries would appear in the output if not explicitly deleted. They don't match any of the substitution patterns so they pass through unchanged

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Match literal `[` or `]` | `\[` `\]` | `/\[DEBUG\]/d` |
| Match literal `(` or `)` in ERE | `\(` `\)` | `\([0-9]{4}\)` |
| Create a capture group in ERE | `(` `)` | `([0-9]{4})` |
| Delete entire matching line | `/pattern/d` | `/\[INFO\]/d` |
| Delete blank lines | `/^$/d` | cleans empty lines |
| Reorder date parts | 4 capture groups + `\4 \3-\2-\1` | YYYY-MM-DD → DD-MM-YYYY |
| Simple tag lowercase | `s/\[TAG\]/[tag]/` | no `-E` needed |
| Timestamp parts need `-E` | always for `{n}` and `()` | `([0-9]{4})` |

> 💡 **Golden Rule — Escaping `(` in ERE:**
> In ERE (`-E`), there are TWO kinds of parentheses:
> ```
> (  )   →  regex capture group  →  saves text as \1, \2...
> \( \)  →  literal parenthesis  →  matches actual ( ) in the data
> ```
> Look at your data — if the `(` is actually in the file, escape it.
> If you need to capture text, use plain `(` without escape.
