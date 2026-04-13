# Extract TODO Comments using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Searches for lines containing `# TODO:`
* Extracts only the **task part** after `# TODO:`
* Prints only the extracted content, nothing else

---

## 📥 Sample Input

```text
        if self.propagate_exceptions:
            # if we want to repropagate the exception, we can attempt to
            # raise it with the whole traceback in case we can do that
            # (the function was actually called from the except part)
            # otherwise, we just raise the error again
            if exc_value is e:
                reraise(exc_type, exc_value, tb)
            else:
                raise e
        self.log_exception((exc_type, exc_value, tb))
        server_error = InternalServerError()
        # TODO: pass as param when Werkzeug>=1.0.0 is required
        # TODO: also remove note about this from docstring and docs
        server_error.original_exception = e
        handler = self._find_error_handler(server_error)
```

---

## 📤 Sample Output

```text
pass as param when Werkzeug>=1.0.0 is required
also remove note about this from docstring and docs
```

---

## ✅ Solution — CLI

```bash
sed '/# TODO:/!d; s/.*# TODO: //'
```

## ✅ Solution — script.sed

```sed
/# TODO:/!d
s/.*# TODO: //
```

```bash
sed -f script.sed input.txt
```

> ⚠️ **Note:** `-E` is **not required** here. No ERE-specific syntax (`{}`, `()`, `|`) is used.
> Plain BRE handles `.*` and literal strings perfectly. `-E` can be added but does nothing extra.

---

## 🔍 Explanation

### How it works — trace for all relevant lines

```
Line: "            # if we want to repropagate..."
      /# TODO:/!d  →  no "# TODO:" found  →  DELETED ✗

Line: "            if exc_value is e:"
      /# TODO:/!d  →  no "# TODO:" found  →  DELETED ✗

Line: "        # TODO: pass as param when Werkzeug>=1.0.0 is required"
      /# TODO:/!d  →  "# TODO:" found  →  NOT deleted, survives
      s/.*# TODO: //
         .*        matches "        "  (leading spaces)
         # TODO:   matches "# TODO: "
         together: "        # TODO: "  is deleted
      Remaining: "pass as param when Werkzeug>=1.0.0 is required"  ✓

Line: "        # TODO: also remove note about this from docstring and docs"
      /# TODO:/!d  →  "# TODO:" found  →  NOT deleted, survives
      s/.*# TODO: //  →  strips prefix
      Remaining: "also remove note about this from docstring and docs"  ✓

Line: "        server_error.original_exception = e"
      /# TODO:/!d  →  no "# TODO:" found  →  DELETED ✗
```

---

### 🔹 Step 1 — Delete Non-TODO Lines

```bash
/# TODO:/!d
```

* `/# TODO:/` → matches lines containing the exact string `# TODO:`
* `!` → negation — inverts the address
* `d` → delete
* Combined: **delete every line that does NOT contain `# TODO:`**
* After this step, only TODO lines remain in the stream

```
All lines
├── contains "# TODO:"  →  KEPT (survives this command)
└── does NOT contain    →  DELETED by !d
```

> This is the **filter-then-transform** pattern — always filter first so
> the expensive substitution only runs on lines you actually care about.

---

### 🔹 Step 2 — Strip Everything Before and Including `# TODO: `

```bash
s/.*# TODO: //
```

* `.*` → matches any characters greedily — captures all leading whitespace/indentation
* `# TODO: ` → matches the literal marker (note the **space after the colon**)
* Together `.*# TODO: ` matches everything from the start up to and including the marker
* Replacement is empty `//` → the entire prefix is deleted
* What remains is only the task text

```
"                    # TODO: pass as param when Werkzeug>=1.0.0 is required"
 └──────────────────────────┘└────────────────────────────────────────────┘
       deleted by .*# TODO:          this survives
```

> **The trailing space matters.** `# TODO:` without a space would leave a
> leading space in the output. The pattern `# TODO: ` (with space) strips
> the space cleanly.

---

### 🔹 Why This is a Two-Step Filter-Then-Transform

You could write it as a single substitution:

```bash
# Alternative: single substitution with -n and p
sed -n 's/.*# TODO: //p' input.txt
```

| Approach | How it works |
|---|---|
| `!d` then `s/...//` | Filter first, then transform survivors |
| `-n` + `s/...//p` | Attempt substitution on every line; print only if it succeeded |

Both produce identical output. The two-step version makes the logic easier to read — filter intent is explicit.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Regex address | `/# TODO:/` | Selects lines containing the exact string |
| 2 | Negation + delete | `!d` | Deletes all lines NOT matching the address |
| 3 | Greedy wildcard | `.*` | Matches any characters from start up to the last occurrence of what follows |
| 4 | Literal string match | `# TODO: ` | Matches this exact sequence including the space |
| 5 | Delete by empty replacement | `s/pattern//` | Removes matched portion — no replacement text |
| 6 | Filter-then-transform | `!d` before `s///` | Ensures substitution only runs on relevant lines |

---

## ⚠️ Important Points

* **`-E` is NOT needed** — no ERE syntax is used. `.*` and literal strings work in default BRE. The problem statement labels it ERE but the solution requires none of it
* **Order is mandatory** — `s/.*# TODO: //` must come after `/# TODO:/!d`. If reversed, the substitution runs on all lines (most of which don't match), and the `!d` then deletes lines that no longer contain `# TODO:` after the substitution — producing empty output
* **The space after `:` in `# TODO: ` is intentional** — it strips the separator space so the output is clean text without a leading space
* **`.*` is greedy** — in `s/.*# TODO: //`, if a line somehow had `# TODO:` twice, the greedy `.*` would consume up to the LAST `# TODO: `, keeping only the final task. For typical code this is the desired behaviour

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Keep only lines matching a pattern | `/pattern/!d` | `/# TODO:/!d` |
| Strip a known prefix from a line | `s/.*PREFIX //` | `s/.*# TODO: //` |
| Filter then transform | `!d` first, `s///` second | Always filter before substituting |
| Print only matching lines | `-n` + `/pattern/p` | `sed -n '/# TODO:/p'` |
| Extract text AND strip prefix in one | `-n` + `s/.*PREFIX //p` | `sed -n 's/.*# TODO: //p'` |
| Case-insensitive match | `I` flag | `/# todo:/Id` |

> 💡 **Golden Rule — Filter Before Transform:**
> Always delete unwanted lines (`!d`) before running substitutions.
> This keeps your script efficient and your logic clear:
> ```
> Step 1: /pattern/!d    ← throw away what you don't need
> Step 2: s/prefix//     ← clean up what you kept
> ```
> This "filter → transform" pattern appears constantly in real sed scripts
> for log parsing, code analysis, and data extraction.
