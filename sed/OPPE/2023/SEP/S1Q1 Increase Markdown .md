# Increase Markdown Heading Level using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Increases the heading level by **1** (adds one `#` at the beginning of heading lines)
* Applies only to lines that start with `#`
* Adds the line `# Text editors in Linux` at the **beginning of the file**

---

## 📥 Sample Input

```text
# Line Editors
## Ed
---
# Modal Editors
## Vi
## Vim
## Neovim
---
# Non-modal Editors
## Nano
## Emacs
```

---

## 📤 Sample Output

```text
# Text editors in Linux
## Line Editors
### Ed
---
## Modal Editors
### Vi
### Vim
### Neovim
---
## Non-modal Editors
### Nano
### Emacs
```

---

## ✅ Solution — CLI

```bash
sed '1i # Text editors in Linux
/^#/ s/^/#/'
```

## ✅ Solution — script.sed

```sed
1i # Text editors in Linux
/^#/ s/^/#/
```

```bash
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — full trace

```
BEFORE any cycle:
  1i fires → outputs "# Text editors in Linux" directly to stdout
  (this text is NEVER loaded into pattern space)

Cycle 1 — line: "# Line Editors"
  /^#/ → starts with # → MATCH
  s/^/#/ → prepend # → "## Line Editors"  ✓

Cycle 2 — line: "## Ed"
  /^#/ → starts with # → MATCH
  s/^/#/ → prepend # → "### Ed"  ✓

Cycle 3 — line: "---"
  /^#/ → does NOT start with # → NO MATCH
  s/^/#/ → SKIPPED
  "---" printed unchanged  ✓

Cycle 4 — line: "# Modal Editors"
  /^#/ → MATCH → "## Modal Editors"  ✓
...
```

---

### 🔹 Insert at Line 1 — `1i text`

```bash
1i # Text editors in Linux
```

* `1` → address — applies only when processing line 1
* `i` → insert command — outputs the text **before** the addressed line
* The inserted text goes **directly to stdout** — it is never loaded into the pattern space

> 🔑 **Critical behaviour:** The `i` command outputs its text as a raw print,
> completely outside the normal Read→Execute→Display cycle.
> This means the inserted line `# Text editors in Linux` is
> **never seen by** `/^#/ s/^/#/` — it cannot be accidentally modified.

```
Without this protection, you might expect:
  1i inserts "# Text editors in Linux"
  /^#/ s/^/#/ runs on it → "## Text editors in Linux"  ✗ WRONG

But actually:
  1i text goes to stdout immediately, bypasses all other commands
  "# Text editors in Linux" printed as-is  ✓ CORRECT
```

---

### 🔹 Address — `/^#/`

```bash
/^#/ s/^/#/
```

* `/^#/` → regex address — selects lines that **start with `#`**
* `^` → anchors to start of line — ensures `#` must be the first character
* This correctly matches `#`, `##`, `###` (any heading level)
* Lines like `---`, `## Ed`, and indented text are handled correctly

```
"# Line Editors"    → starts with # → ✓ selected
"## Ed"             → starts with # → ✓ selected
"---"               → starts with - → ✗ skipped
" # indented"       → starts with space, not # → ✗ skipped
```

---

### 🔹 Add One `#` — `s/^/#/`

```bash
s/^/#/
```

* `^` → zero-width match at the start of the line
* `/#/` → replaces that position with `#`
* Effectively **prepends** one `#` to the line
* No original content is removed — `^` is a position, not a character

```
"# Line Editors"   → s/^/#/ → "## Line Editors"
"## Ed"            → s/^/#/ → "### Ed"
"### Deep"         → s/^/#/ → "#### Deep"
```

> This is the same `s/^/prefix/` trick used in the periodic labels question.
> `^` is a zero-width anchor — substituting it inserts without deleting.

---

### 🔹 Why Order Matters — `1i` Before `/^#/`

```
Command order in script:
  Line 1:  1i # Text editors in Linux   ← insert FIRST
  Line 2:  /^#/ s/^/#/                  ← modify SECOND
```

If the order were reversed:
```
/^#/ s/^/#/         ← modifies all heading lines including line 1
1i # Text editors   ← inserts header AFTER line 1 is already modified
```

The output would be identical in this specific case because `1i` output is
independent of the pattern space. But logically, insert-first is the correct
and readable order — it reflects the intended structure.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Insert before line | `Ni text` | Outputs text before line N, bypasses pattern space |
| 2 | Regex address | `/^#/` | Selects only lines beginning with `#` |
| 3 | Start anchor | `^` | Matches position at start of line (zero-width) |
| 4 | Prepend via substitution | `s/^/text/` | Inserts text at the beginning of the line |
| 5 | Address + command | `/pattern/ command` | Runs command only on lines matching pattern |

---

## ⚠️ Important Points

* **`i` text bypasses the pattern space entirely** — the inserted line `# Text editors in Linux` is never processed by `/^#/ s/^/#/`. It is safe from accidental modification
* **`/^#/` not `/^#+/`** — both work here, but `/^#/` is sufficient since any line starting with `#` is a heading regardless of how many `#` symbols follow
* **`s/^/#/` has no `g` flag** — `^` can only match once per line (there is only one start-of-line). Adding `g` would make no difference
* **Non-heading lines (`---`) are fully untouched** — the `/^#/` address guards the substitution, so `---` lines never enter the `s/^/#/` command

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|---|---|---|
| Insert a line before line 1 | `1i text` | `1i # Title` |
| Insert a line after the last line | `$a text` | `$a ## Footer` |
| Insert before every matching line | `/pattern/i text` | `/^#/i ---` |
| Add text at start of matching lines | `/pattern/ s/^/text/` | `/^#/ s/^/#/` |
| Add text at end of matching lines | `/pattern/ s/$/text/` | `/^#/ s/$/ ##/` |
| Protect inserted text from modification | Use `i` — it bypasses pattern space | `1i safe text` |
| Match any heading regardless of level | `/^#/` | matches `#`, `##`, `###` |
| Match only level-1 headings | `/^# /` (with space) | `# Title` but not `## Sub` |

> 💡 **Golden Rule — `i` command is output-only:**
> Text inserted by `i` (and `a`) is sent directly to stdout.
> It is **never loaded into the pattern space** and therefore
> **cannot be matched or modified** by any other command in the script.
> Use this when you need to add fixed header/footer lines
> that must not be affected by your transformation rules.
