# Visualize Tabs and Line Endings using sed

---

## 📝 Problem Statement

A student wants to visualize hidden characters in a file.

Write a **sed script** that:
* Replaces every **tab character** with `^I`
* Marks the **end of each line** with `$`

---

## 📥 Sample Input

```text
Hello	World
This	is	a	test
Line without tab
```

*(Note: tabs are present between words)*

---

## 📤 Sample Output

```text
Hello^IWorld$
This^Iis^Ia^Itest$
Line without tab$
```

---

## ✅ Solution — CLI

```bash
sed 's/\t/^I/g; s/$/$/'
```

## ✅ Solution — script.sed

```sed
s/\t/^I/g
s/$/$/
```

```bash
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — trace for `This	is	a	test`

```
Input:   This→is→a→test          (→ represents a real tab character)

Step 1 — s/\t/^I/g
         \t matches every tab character in the line
         Each tab is replaced with the two visible characters ^I
         Remaining: This^Iis^Ia^Itest

Step 2 — s/$/$/
         $ matches the end-of-line position (after the last character)
         A literal $ character is inserted at that position
         Remaining: This^Iis^Ia^Itest$  ✓
```

---

### 🔹 Step 1 — Replace Every Tab with `^I`

```bash
s/\t/^I/g
```

* `\t` → matches a **literal tab character** (ASCII 9) — the invisible whitespace used for alignment
* `^I` → the replacement — two visible characters (`^` and `I`) that is the traditional Unix notation for a tab
* `/g` → the **global flag** — without it, only the first tab on each line would be replaced; with it, every tab on the line is replaced

```
This→is→a→test          (raw, tabs invisible)
     ↓  ↓  ↓
     Each \t matched and replaced

This^Iis^Ia^Itest       (all tabs now visible as ^I)
```

> **Why `^I`?** In terminal control notation, Ctrl+I produces a tab character. The caret `^` represents the Ctrl key, and `I` is the key pressed — so `^I` is the human-readable name for tab.

---

### 🔹 Step 2 — Append `$` at End of Line

```bash
s/$/$/
```

* `$` in the **pattern** → matches the **end-of-line position** — it is a zero-width anchor, meaning it does not consume any character, it only marks the position after the last character
* `$` in the **replacement** → inserts the literal `$` character at that position
* The result is that every line gets a `$` appended visually, marking exactly where the line ends

```
This^Iis^Ia^Itest        ← no visible end marker
                 ↑
            $ anchor matches here (end of line position)

This^Iis^Ia^Itest$       ✓ end of line now visible
```

> **Why does `s/$/$/` work?** It looks odd because `$` appears on both sides. On the left it is a regex anchor (position), on the right it is a literal character. sed treats them differently depending on which side of `/` they appear — left side is a pattern, right side is a plain replacement string.

---

### 🔹 Why No `-E` Flag is Needed

```bash
sed 's/\t/^I/g; s/$/$/'
```

Neither `\t` nor `$` requires extended regex (`-E`). Both are part of basic regex (BRE) supported by sed universally. The `-E` flag only becomes necessary when using extended features like `+`, `?`, `|`, or `()` grouping. This solution intentionally avoids `-E` to stay maximally portable.

---

### 🔹 Why Two Steps and Not One?

You could attempt to handle both in a single substitution, but it becomes awkward. The two operations target completely different things:

| Step | Targets | Nature |
|---|---|---|
| `s/\t/^I/g` | Tab characters (actual content) | Character replacement |
| `s/$/$/` | End-of-line position (zero-width) | Anchor-based insertion |

There is no single pattern that cleanly handles both in one pass. The two-step approach is the natural, readable, and correct solution.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Tab escape sequence | `\t` | Matches the invisible tab character (ASCII 9) |
| 2 | Global flag | `g` | Replaces all matches on a line, not just the first |
| 3 | End-of-line anchor | `$` in pattern | Zero-width match at the position after the last character |
| 4 | Literal replacement | `$` in replacement | Inserts a real `$` character into the output |
| 5 | Chained substitutions | `s/a//; s/b//` | Two `s` commands run sequentially on the same line |
| 6 | No `-n` needed | *(default print)* | sed prints every processed line automatically |

---

## ⚠️ Important Points

* **`\t` is a character, `$` is a position** — `\t` matches an actual byte in the line; `$` matches a location between the last character and the newline. This distinction explains why `g` is needed for `\t` (multiple tabs possible) but not for `$` (there is exactly one end of line per line)
* **`$` on left vs right side of `s///` means different things** — in `s/$/$/`, the left `$` is the regex anchor (zero-width), and the right `$` is a literal dollar sign character being inserted. sed distinguishes them by position
* **Without `g`, only the first tab is replaced** — if a line has three tabs and you use `s/\t/^I/` without `g`, only the first tab becomes `^I` and the rest stay invisible
* **This mimics `cat -A`** — the Unix command `cat -A` does exactly this: shows `^I` for tabs and `$` for line endings. Writing it in sed teaches you how that display works internally
* **The output `$` is not a newline** — sed still adds a newline when printing the line; the `$` you see in the output is a visual marker inserted before the newline, not a replacement for it

---

## 🧠 Memory — Visualizing Hidden Characters

| Hidden Character | Visible Notation | sed Command | Why `/g`? |
|---|---|---|---|
| Tab (`\t`) | `^I` | `s/\t/^I/g` | ✅ Yes — multiple tabs per line possible |
| End of line | `$` | `s/$/$/` | ❌ No — only one end of line per line |
| Both together | `^I` and `$` | Chain both commands | Apply tab first, then end marker |

> 💡 **Golden Rule — Making Hidden Characters Visible:**
> Every hidden or non-printing character has a traditional visible notation.
> To reveal it with sed, simply substitute the escape sequence for that character with its visible representation:
> 1. `s/\t/^I/g` → every tab becomes the two-character string `^I`
> 2. `s/$/$/` → the invisible end-of-line position gets a literal `$` stamped on it
>
> This "invisible → visible" pattern is the foundation of how tools like `cat -A` and `sed -A` work under the hood.
