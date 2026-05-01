# Log Filtering and Normalization using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Deletes all lines containing `DEBUG`
* Replaces all timestamps of the form `[HH:MM:SS]` with `[TIME]`
* Collapses consecutive duplicate words into a single occurrence (at most 3 repetitions)
* Uses no branching (no `:`, `b`, `t` commands)
* Applies operations in the given order

---

## 📥 Sample Input

```text
[12:45:09] INFO Server started
[12:45:10] DEBUG Connection pool initialized
[12:45:11] ERROR ERROR Database unreachable
[12:45:12] WARN WARN WARN Disk usage high
[12:45:13] DEBUG Cache cleared
[12:45:14] INFO INFO Request processed
[12:45:15] ERROR Timeout occurred
```

---

## 📤 Sample Output

```text
[TIME] INFO Server started
[TIME] ERROR Database unreachable
[TIME] WARN Disk usage high
[TIME] INFO Request processed
[TIME] ERROR Timeout occurred
```

---

## ✅ Solution — CLI

```bash
sed -E '
/DEBUG/d
s/\[[0-9]{2}:[0-9]{2}:[0-9]{2}\]/[TIME]/g
s/\b([A-Za-z]+) \1\b/\1/g
s/\b([A-Za-z]+) \1\b/\1/g
' input.txt
```

## ✅ Solution — script.sed

```sed
/DEBUG/d
s/\[[0-9]{2}:[0-9]{2}:[0-9]{2}\]/[TIME]/g
s/\b([A-Za-z]+) \1\b/\1/g
s/\b([A-Za-z]+) \1\b/\1/g
```

```bash
sed -E -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — step-by-step pipeline trace

After Step 1 (Delete all DEBUG lines — `/DEBUG/d`):
```
[12:45:09] INFO Server started
[12:45:11] ERROR ERROR Database unreachable
[12:45:12] WARN WARN WARN Disk usage high
[12:45:14] INFO INFO Request processed
[12:45:15] ERROR Timeout occurred
```
Lines 2 and 5 (containing "DEBUG") deleted. 5 lines remain.

After Step 2 (Normalize timestamps — `s/\[...\]/[TIME]/g`):
```
[TIME] INFO Server started
[TIME] ERROR ERROR Database unreachable
[TIME] WARN WARN WARN Disk usage high
[TIME] INFO INFO Request processed
[TIME] ERROR Timeout occurred
```
All `[HH:MM:SS]` patterns replaced with `[TIME]`.

After Step 3 (First dedup pass — `s/\b([A-Za-z]+) \1\b/\1/g`):
```
[TIME] INFO Server started
[TIME] ERROR Database unreachable        ← "ERROR ERROR" → "ERROR"
[TIME] WARN WARN Disk usage high         ← "WARN WARN WARN" → "WARN WARN" (3→2)
[TIME] INFO Request processed            ← "INFO INFO" → "INFO"
[TIME] ERROR Timeout occurred
```
First pass reduces 2→1 and 3→2. Triple "WARN" still has one duplicate left.

After Step 4 (Second dedup pass — same command again):
```
[TIME] INFO Server started
[TIME] ERROR Database unreachable
[TIME] WARN Disk usage high              ← "WARN WARN" → "WARN" (2→1) ✓
[TIME] INFO Request processed
[TIME] ERROR Timeout occurred
```
Second pass catches the remaining duplicate. All duplicates now collapsed. ✓

---

### 🔹 Delete DEBUG Lines — `/DEBUG/d`

```sed
/DEBUG/d
```

* `/DEBUG/` → address pattern — matches any line containing the substring `DEBUG` anywhere
* `d` → **delete** command — removes the matched line entirely and moves to the next input line

Input:  `"[12:45:10] DEBUG Connection pool initialized"`
Match:  Line contains "DEBUG" → YES
Result: Line deleted, not printed to output

Input:  `"[12:45:09] INFO Server started"`
Match:  Line contains "DEBUG" → NO
Result: Line passes through unchanged

> 💡 `d` deletes the **entire line**, not just the matched word. The line is removed from the output completely. All subsequent sed commands are **skipped** for a deleted line — sed immediately moves to the next input line.

---

### 🔹 Normalize Timestamps — `s/\[[0-9]{2}:[0-9]{2}:[0-9]{2}\]/[TIME]/g`

```sed
s/\[[0-9]{2}:[0-9]{2}:[0-9]{2}\]/[TIME]/g
```

**Search side (regex):**
* `\[` → literal `[` character (escaped because `[` starts a character class in regex)
* `[0-9]{2}` → exactly 2 digits (character class `[0-9]`, quantifier `{2}` enabled by `-E`)
* `:` → literal colon (not special in regex)
* `\]` → literal `]` character (escaped for clarity)
* Pattern matches: `[12:45:09]`, `[00:00:00]`, `[23:59:59]`, etc.

**Replacement side:**
* `[TIME]` → literal text — `[` and `]` are **NOT special** on the replacement side, no escaping needed

**Flags:**
* `g` → global — replace **all** occurrences on the line (in case a line has multiple timestamps)

Input:  `"[12:45:11] ERROR ERROR Database unreachable"`
Match:  `[12:45:11]` matches the timestamp pattern
Result: `"[TIME] ERROR ERROR Database unreachable"`

> 💡 **Search-side vs Replacement-side escaping:** On the search side, `\[` and `\]` are needed because `[` and `]` have regex meaning (character classes). On the replacement side, `[TIME]` needs NO escaping because `[` and `]` are just literal characters there. Only `\`, `&`, and `\1`–`\9` are special on the replacement side.

---

### 🔹 Collapse Duplicate Words (Pass 1) — `s/\b([A-Za-z]+) \1\b/\1/g`

```sed
s/\b([A-Za-z]+) \1\b/\1/g
```

**Search side (regex):**
* `\b` → **word boundary** — matches the position between a word character and a non-word character
* `(` → start of capture group 1 (ERE syntax with `-E`, no backslash needed)
* `[A-Za-z]+` → one or more letters — captures a complete word
* `)` → end of capture group 1
* ` ` → literal space between the two words
* `\1` → **backreference** on search side — matches the exact same text captured by group 1
* `\b` → word boundary after the second occurrence

**Replacement side:**
* `\1` → inserts the text from capture group 1 (one copy of the word)

**Flags:**
* `g` → global — replace all occurrences on the line

Input:  `"[TIME] ERROR ERROR Database unreachable"`
Match:  `\b(ERROR) ERROR\b` → group 1 captures "ERROR"
Result: `"[TIME] ERROR Database unreachable"`

Input:  `"[TIME] WARN WARN WARN Disk usage high"`
Match:  `\b(WARN) WARN\b` → first pair matched, replaced with "WARN"
Result: `"[TIME] WARN WARN Disk usage high"` (third WARN remains — needs second pass)

> 💡 Why doesn't one pass handle "WARN WARN WARN"? The `g` flag continues scanning **after** the replacement position. After replacing the first "WARN WARN" with "WARN", the scanner is past position 4. The remaining " WARN" starts with a space, so the next `\b([A-Za-z]+)` can't start until "WARN" at a later position — but there's only one "WARN" left adjacent, not a pair. So a second pass is needed.

---

### 🔹 Collapse Duplicate Words (Pass 2) — Same command repeated

```sed
s/\b([A-Za-z]+) \1\b/\1/g
```

This is the **exact same command** applied a second time. It catches any remaining duplicates left after Pass 1.

* Pass 1 reduces: 3 → 2, 2 → 1
* Pass 2 reduces: 2 → 1 (catches leftover from 3→2)

Input:  `"[TIME] WARN WARN Disk usage high"` (result from Pass 1)
Match:  `\b(WARN) WARN\b` → matches the remaining duplicate
Result: `"[TIME] WARN Disk usage high"` ✓

> 💡 **Why two passes instead of branching?** Normally, sed uses `t` (branch on successful substitution) to loop a command until no more matches. But the problem forbids branching. Since the max repetition is 3, exactly **two passes** handle all cases: Pass 1 (3→2, 2→1) + Pass 2 (2→1). For max 4 repetitions, you'd need 3 passes, and so on.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---------|--------|--------------|
| 1 | Delete command | `/pattern/d` | Removes entire line matching pattern |
| 2 | Substitution | `s/regex/replacement/g` | Replaces all matches of regex with replacement |
| 3 | ERE mode | `sed -E` | Enables extended regex: `+`, `{}`, `()` without backslashes |
| 4 | Escaped literal `[` | `\[` | Matches a literal `[` on the search side |
| 5 | Character class | `[0-9]` | Matches any single digit |
| 6 | Quantifier | `{2}` | Matches exactly 2 of the preceding element |
| 7 | Capture group | `([A-Za-z]+)` | Captures matched text for backreference |
| 8 | Backreference (search) | `\1` in search | Matches same text as capture group 1 |
| 9 | Backreference (replace) | `\1` in replacement | Inserts text from capture group 1 |
| 10 | Word boundary | `\b` | Matches boundary between word and non-word character |
| 11 | Global flag | `g` | Replaces all occurrences, not just the first |
| 12 | Multi-pass dedup | Same `s///g` twice | Handles up to 3 consecutive repetitions without branching |

---

## ⚠️ Important Points

* **`d` skips all remaining commands** — When `/DEBUG/d` matches, sed deletes the line and immediately moves to the next input line. The timestamp and dedup commands are never executed for that line. This is why `/DEBUG/d` must come **first**.

* **`\[` on search side vs `[` on replacement side** — In the regex `s/\[...\]/[TIME]/g`, the search side needs `\[` because `[` starts a character class. The replacement `[TIME]` needs NO escaping because `[` is literal on the replacement side. Mixing these up is a very common mistake.

* **`\1` means different things on each side** — On the **search side**, `\1` is a backreference that **matches** the same text as group 1. On the **replacement side**, `\1` **inserts** the captured text. Same syntax, different behavior.

* **Two passes are needed for triple repetitions** — With `g` flag, `s/\b(WORD) WORD\b/WORD/g` processes left-to-right. After replacing the first pair in "A A A", the scanner is past the replacement, leaving "A A". A second identical command catches this. Without branching, manual repetition is the only option.

* **`\b` prevents partial word matches** — Without `\b`, the pattern `([A-Za-z]+) \1` could match substrings across word boundaries. For example, in "ERRORS ERROR", it might match "RROR RROR" (partial). `\b` ensures only complete words are matched.

* **`-E` flag is critical** — Without `-E`, you'd need `\+`, `\{2\}`, `\(` and `\)` instead of `+`, `{2}`, `(` and `)`. Forgetting `-E` while using ERE syntax causes silent failures — sed treats them as literal characters.

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|-----------|----------|---------|
| Remove entire lines matching a pattern | `/pattern/d` | `/DEBUG/d` |
| Replace text with something else | `s/old/new/g` | `s/\[..\]/[TIME]/g` |
| Match literal `[` or `]` in regex | `\[` and `\]` | `s/\[time\]/[TIME]/` |
| Match exact N repetitions | `{N}` with `-E` | `[0-9]{2}` = exactly 2 digits |
| Capture text for reuse | `(pattern)` with `-E` | `([A-Za-z]+)` |
| Reference captured text in search | `\1` on search side | `([A-Za-z]+) \1` = duplicate word |
| Insert captured text in replacement | `\1` on replacement side | `s/(word) \1/\1/` |
| Ensure whole-word matching | `\b` word boundary | `\b([A-Za-z]+) \1\b` |
| Replace all matches on a line | `g` flag | `s/old/new/g` |
| Handle N-deep repetitions without branching | Repeat `s///g` N-1 times | 3 repeats → 2 identical `s///g` lines |

> 💡 **Golden Rule — Search-Side vs Replacement-Side Escaping:**
> ```
> SEARCH SIDE:  Special chars need escaping for literal use
>   \[  → literal [     (because [ starts character class)
>   \]  → literal ]
>   \1  → backreference  (matches same text as group 1)
>
> REPLACEMENT SIDE:  Almost everything is literal
>   [   → literal [     (no escaping needed!)
>   ]   → literal ]
>   \1  → inserts group 1 text
>   &   → inserts entire match
>
> Example:  s/\[([0-9]+)\]/[\1]/g
>           Search:  \[ escapes [    \] escapes ]
>           Replace: [ is literal    ] is literal    \1 inserts capture
> ```
