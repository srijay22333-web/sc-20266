# Log File Transformation using sed — Delete + Uppercase + Timestamp

---

## 📝 Problem Statement

Write a **sed script** that:
* Deletes `[DEBUG]` and `[INFO]` log lines
* Rewrites timestamps from `YYYY-MM-DD HH:MM:SS` → `HH:MM:SS DD-MM-YYYY`
* Uppercases the **first letter of the message** (the part after the timestamp)

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
[WARNING] (12:02:00 15-03-2023) This is a warning message
[ERROR] (12:03:00 15-03-2023) This is an error message
[CRITICAL] (12:04:00 15-03-2023) This is a critical message
```

---

## ✅ Solution — CLI

```bash
sed -E '
/\[DEBUG\]/d
/\[INFO\]/d
s/\(([0-9]{4})-([0-9]{2})-([0-9]{2}) ([0-9]{2}:[0-9]{2}:[0-9]{2})\)/(\4 \3-\2-\1)/
s/[^) ][^)]+$/\u&/
' input.txt
```

## ✅ Solution — script.sed

```sed
/\[DEBUG\]/d
/\[INFO\]/d
s/\(([0-9]{4})-([0-9]{2})-([0-9]{2}) ([0-9]{2}:[0-9]{2}:[0-9]{2})\)/(\4 \3-\2-\1)/
s/[^) ][^)]+$/\u&/
```

```bash
sed -E -f script.sed input.txt
```

---

## 🔍 Explanation

### Step-by-step pipeline trace

```
Input:  [WARNING] (2023-03-15 12:02:00) this is a warning message

After /\[DEBUG\]/d and /\[INFO\]/d:
  → line survives (it's WARNING)

After timestamp rewrite:
  [WARNING] (12:02:00 15-03-2023) this is a warning message

After s/[^) ][^)]+$/\u&/:
  [WARNING] (12:02:00 15-03-2023) This is a warning message  ✓
```

---

### 🔹 Delete Lines — `/\[DEBUG\]/d` and `/\[INFO\]/d`

```bash
/\[DEBUG\]/d
/\[INFO\]/d
```

* `\[` and `\]` → escape brackets to match them as **literals** (unescaped `[` starts a character class)
* `d` → deletes the line; no further commands run on it
* The remaining 3 line types (WARNING, ERROR, CRITICAL) pass through untouched

---

### 🔹 Timestamp Rewrite — 4 Capture Groups

```bash
s/\(([0-9]{4})-([0-9]{2})-([0-9]{2}) ([0-9]{2}:[0-9]{2}:[0-9]{2})\)/(\4 \3-\2-\1)/
```

```
\(                    → literal ( in the data
  ([0-9]{4})  = \1    → YYYY
  -
  ([0-9]{2})  = \2    → MM
  -
  ([0-9]{2})  = \3    → DD
  (space)
  ([0-9]{2}:[0-9]{2}:[0-9]{2}) = \4  → HH:MM:SS
\)                    → literal ) in the data

Replacement: (\4 \3-\2-\1)
  = (HH:MM:SS DD-MM-YYYY)
  = (12:02:00 15-03-2023)  ✓
```

*(This is identical to the previous question — refer to that for the full breakdown of `\(` vs `(`)*

---

### 🔹 The Smart Uppercase Trick — `s/[^) ][^)]+$/\u&/`

This is the most interesting part. It must uppercase the first letter of the **message only**, without touching `[WARNING]`, the timestamp, or any other part.

```
[WARNING] (12:02:00 15-03-2023) this is a warning message
                                ^
                                must uppercase only this 't'
```

**The pattern:** `[^) ][^)]+$`

```
[^) ]    → one character that is NOT ')' and NOT space
[^)]+    → one or more characters that are NOT ')'
$        → anchored to end of line
```

**Why this cleverly skips `[WARNING]` and the timestamp:**

The regex engine tries every position left to right, looking for where `[^) ][^)]+$` can match all the way to `$`. It must find a starting position where NO `)` appears between the start and the end of line.

```
pos  0  '['  → [^) ] ✓, but [^)]+$ hits ')' in "15-03-2023)" → FAIL
pos  1  'W'  → [^) ] ✓, but [^)]+$ hits ')' later            → FAIL
...
pos 10  '('  → [^) ] ✓, but [^)]+$ hits ')' in timestamp     → FAIL
pos 11  '1'  → [^) ] ✓, but [^)]+$ hits ')' after 2023       → FAIL
...
pos 29  '3'  → last digit of "2023", [^)]+$ hits ')'          → FAIL
pos 30  ')'  → [^) ] FAILS immediately (it IS ')')
pos 31  ' '  → [^) ] FAILS immediately (it IS space)
pos 32  't'  → [^) ] ✓, [^)]+$ = "his is a warning message"
               no ')' from here to end of line                  → MATCH ✓
```

**The key insight:** Every position before the message fails because the timestamp `)` always appears between that position and the end of line. The message is the only section with **no `)` between it and `$`**.

```
[WARNING] (12:02:00 15-03-2023) this is a warning message
           ↑               ↑   ↑
           (               )   match starts here — no ) after this
```

**Replacement:** `\u&`

* `&` → backreference to the **entire match** = `this is a warning message`
* `\u` → uppercase the **very next character** only
* Combined: first char of `&` is uppercased, rest stays as-is

```
Match:   "this is a warning message"
\u&  →   "This is a warning message"  ✓
```

---

### 🔹 Why Not Use a Simpler Pattern for Uppercase?

You might ask — why not just:
```bash
s/) \(.\)/)\u \1/     # BRE
s/\) (.)/\) \u\1/     # ERE — also works
```

Both also work:
```
") t" → ") T"  ✓
```

| Approach | Pattern | Readable? | Robust? |
|---|---|---|---|
| After `) ` | `s/\) (.)/\) \u\1/` | ✅ Clear intent | ✅ Explicit |
| After last `)` | `s/[^) ][^)]+$/\u&/` | ⚠️ Tricky | ✅ Works by structure |

The `[^) ][^)]+$` approach is more clever — it works because of the line's **structural property** (only the message has no `)` until end of line), not by explicitly targeting `) `.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Escaped brackets | `\[` `\]` | Match literal `[` `]` in log tags |
| 2 | Delete command | `d` | Removes matching lines entirely |
| 3 | Timestamp capture | `([0-9]{4})` × 4 | Captures YYYY MM DD HH:MM:SS separately |
| 4 | Escaped literal parens | `\(` `\)` | Matches `(` `)` around the timestamp |
| 5 | Negated class | `[^)]` | Matches any char except `)` |
| 6 | End anchor | `$` | Forces match to reach end of line |
| 7 | Whole match reference | `&` | Recalls the entire matched text |
| 8 | First-char uppercase | `\u` | Uppercases only the next character |
| 9 | ERE flag | `-E` | Enables `{n}` and `()` without backslashes |

---

## ⚠️ Important Points

* **`[^) ]` excludes both `)` and space** — this prevents matching at the space between `)` and the message. Without excluding space, the space at position 31 could start a match
* **`[^)]+$` is the gatekeeper** — the `$` at the end forces the engine to find a segment with **zero `)` characters all the way to end of line**. Only the message part satisfies this
* **`\u&` uppercases only the first character** — `\u` is a one-shot modifier. It uppercases exactly the next character. The rest of `&` is printed as-is
* **Timestamp rewrite must happen BEFORE the uppercase step** — after the rewrite, `)` appears at the end of the timestamp section. The `[^)]+$` trick depends on this `)` being present to block earlier matches

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Uppercase first char of a known position | `\) (.)/\) \u\1/` | after `) ` |
| Uppercase first char of "last field" | `[^DELIM][^DELIM]+$/\u&/` | after last delimiter |
| `\u` uppercases how many chars? | Only **1** — the very next char | `\u&` → first of `&` |
| `\U` uppercases how many chars? | All that follow until `\E` | `\U&` → whole match |
| Match "everything after last X" | `[^X]+$` | `[^)]+$` |
| Avoid matching at a space | Add space to negated class `[^X ]` | `[^) ]` |
| Whole match in replacement | `&` | `s/.*/PREFIX_&/` |

> 💡 **Golden Rule — `[^) ][^)]+$` trick:**
> This pattern finds the **last segment of a line that contains no `)` characters**.
> It works because:
> 1. Every earlier position fails — the `)` from the timestamp blocks `[^)]+$` from reaching `$`
> 2. Only the message (after the final `)`) has a clear path to end of line
>
> This is a **structural trick** — it relies on the log format's guarantee
> that `)` only appears inside the timestamp parentheses, never in the message.
