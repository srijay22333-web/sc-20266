# Convert Case Based on Prefix using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Reads input lines in the format `prefix:text`
* If the prefix is `low` → converts the text to **lowercase** and prints it
* If the prefix is `up` → converts the text to **uppercase** and prints it
* Prints **only the converted text** — the prefix and colon are removed

---

## 📥 Sample Input

```text
low:This is a Low Case Message
up:This is an Up Case Message
```

---

## 📤 Sample Output

```text
this is a low case message
THIS IS AN UP CASE MESSAGE
```

---

## ✅ Solution — CLI

```bash
sed -E 's/^low:(.*)/\L\1/; s/^up:(.*)/\U\1/'
```

## ✅ Solution — script.sed

```sed
s/^low:(.*)/\L\1/
s/^up:(.*)/\U\1/
```

```bash
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — trace for each line

```
Line 1: "low:This is a Low Case Message"

  Step 1 — s/^low:(.*)/\L\1/
    ^low:  → matches the literal prefix at start of line
    (.*)   → captures "This is a Low Case Message" into group \1
    \L\1   → replays \1 with \L applied → converts all to lowercase
    Result: "this is a low case message"  ✓

  Step 2 — s/^up:(.*)/\U\1/
    ^up:   → does NOT match "this is a low case message"
    → no change
    Final:  "this is a low case message"  ✓

─────────────────────────────────────────────────────

Line 2: "up:This is an Up Case Message"

  Step 1 — s/^low:(.*)/\L\1/
    ^low:  → does NOT match "up:..."
    → no change

  Step 2 — s/^up:(.*)/\U\1/
    ^up:   → matches the literal prefix at start of line
    (.*)   → captures "This is an Up Case Message" into group \1
    \U\1   → replays \1 with \U applied → converts all to uppercase
    Result: "THIS IS AN UP CASE MESSAGE"  ✓
```

---

### 🔹 Step 1 — Match and Capture the Text Part

```bash
^low:(.*)
```

This pattern has three distinct parts working together. The `^` anchor ensures the match only succeeds if `low:` appears at the very **start of the line** — this prevents accidentally matching a line like `not-low:text` or `download:file`. The literal string `low:` then matches the exact prefix including the colon. Finally, `(.*)` is a **capture group** that matches and remembers everything after the colon — the entire text content, including spaces and any mixed-case characters.

```
low:This is a Low Case Message
│   │                         │
│   └─── (.*) captures ───────┘
│         stored as \1
└── ^low: anchor + prefix consumed and discarded
```

The same structure applies to `^up:(.*)` — anchored at start, matches the `up:` prefix, captures the rest.

---

### 🔹 Step 2 — Case Conversion in the Replacement

```bash
\L\1
```

This is the replacement string for the `low:` rule. It consists of two parts placed side by side. `\L` is a **GNU sed case conversion escape** that tells sed to convert all following replacement characters to **lowercase** until it encounters a `\U` or `\E` (end of conversion). `\1` is the **backreference** that replays the text captured by the first group. Since `\L` precedes `\1`, every character that `\1` expands to is automatically lowercased as it is written to the output.

```
Replacement:  \L  \1
               │    │
               │    └── expands to "This is a Low Case Message"
               └── \L applies lowercase to everything that follows
               
Output: "this is a low case message"
```

Similarly, `\U\1` in the second rule converts all characters of `\1` to uppercase.

```
Replacement:  \U  \1
               │    │
               │    └── expands to "This is an Up Case Message"
               └── \U applies uppercase to everything that follows

Output: "THIS IS AN UP CASE MESSAGE"
```

> **Important:** `\L` and `\U` are features of the **replacement string** — they are GNU sed extensions and are completely independent of whether you use `-E` or not. They do not belong to the regex engine; they belong to the substitution output processor.

---

### 🔹 Why `^` is Critical

Without the `^` anchor, a line like `this has low: in the middle` would accidentally match `/low:(.*)/` and incorrectly transform it. The `^` pins the match to the start of the line, ensuring only lines whose prefix is exactly `low:` or `up:` are processed.

```
Without ^:
  "download:file.txt"
   ↑ contains "low:" at position 4 → WRONG match
   → would produce "file.txt" converted to lowercase

With ^:
  "download:file.txt"
   → ^low: does not match at position 0 → safe, skipped ✓
```

---

### 🔹 Why `-E` is Used and What It Enables

The `-E` flag activates **Extended Regular Expressions (ERE)**. In this solution it enables the unescaped parentheses syntax `(.*)` for capture groups. In basic regex mode (BRE, without `-E`), you would need to write `\(.*\)` with escaped parentheses. Both work identically — `-E` simply makes the syntax cleaner and more readable.

| Mode | Capture Group Syntax | Case Escape | Works? |
|---|---|---|---|
| BRE (no `-E`) | `\(.*\)` with `\1` | `\L`, `\U` | ✅ Yes |
| ERE (with `-E`) | `(.*)` with `\1` | `\L`, `\U` | ✅ Yes |

The `\L` and `\U` escapes work the same in both modes — `-E` does not affect them at all.

---

### 🔹 GNU sed Portability Note

`\L` and `\U` in the replacement string are **GNU sed extensions**. They are not part of the POSIX sed standard. This solution will work correctly on Linux systems with GNU sed, but may fail on BSD sed (macOS default), Solaris sed, or other POSIX-only implementations. On macOS, you can install GNU sed via `brew install gnu-sed` and invoke it as `gsed`.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Start-of-line anchor | `^` | Ensures prefix is matched only at the beginning of the line |
| 2 | Literal prefix match | `low:` / `up:` | Matches the exact case directive and colon |
| 3 | Capture group (ERE) | `(.*)` | Captures everything after the colon into group `\1` |
| 4 | Backreference | `\1` | Replays the captured text in the replacement |
| 5 | Lowercase conversion | `\L` | Converts all following replacement characters to lowercase (GNU sed) |
| 6 | Uppercase conversion | `\U` | Converts all following replacement characters to uppercase (GNU sed) |
| 7 | Chained substitutions | `s/a//; s/b//` | Two `s` commands run sequentially on the same line |

---

## ⚠️ Important Points

* **`\L` and `\U` are GNU sed only** — they are not POSIX. Scripts using them are not portable to BSD/macOS sed without installing GNU sed
* **`\L` and `\U` apply to everything that follows** — they stay active for the rest of the replacement string unless explicitly stopped with `\E` (end conversion). Since `\1` is the only thing after them here, `\E` is not needed
* **`^` is essential** — without anchoring to the start, any line containing the substring `low:` or `up:` anywhere would be incorrectly matched and transformed
* **Unmatched lines pass through unchanged** — a line starting with `low:` is untouched by the `up:` rule, and vice versa. Lines with any other prefix (e.g., `mid:text`) are untouched by both rules and print as-is
* **`.*` is greedy and matches the entire text including spaces and punctuation** — this is the correct behaviour here since we want to capture and convert the complete text after the colon

---

## 🧠 Memory — Case Conversion Escapes in GNU sed

| Escape | Scope | Effect | Example |
|---|---|---|---|
| `\l` | Next character only | Lowercase one char | `\l\1` → `hELLO` (only h lowercased) |
| `\u` | Next character only | Uppercase one char | `\u\1` → `Hello` (only H uppercased) |
| `\L` | All following chars | Lowercase everything | `\L\1` → `hello world` |
| `\U` | All following chars | Uppercase everything | `\U\1` → `HELLO WORLD` |
| `\E` | Terminator | Stop conversion | `\U\1\E rest` → `HELLO rest` |

> 💡 **Golden Rule — Prefix-Based Transformation:**
> When input lines carry a directive in their prefix that controls how the rest of the line should be processed, the pattern is always:
> 1. **Anchor** with `^` to match the prefix at the start only
> 2. **Capture** the payload with `(.*)`
> 3. **Transform and replay** the capture with `\L\1` or `\U\1`
>
> The prefix is consumed and discarded by the match — only `\1` appears in the replacement, already converted. This "match prefix → transform payload → discard prefix" pattern cleanly separates the control signal from the data in one substitution.
