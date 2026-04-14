# Remove HTML Tags using sed

---

## 📝 Problem Statement

Write a **sed script** that:
* Removes all **HTML tags** from the input
* Keeps only the **text content**
* Assumes that tags **open and close on the same line** (no tag spans multiple lines)

---

## 📥 Sample Input

```text
A <b>table</b> is an arrangement of
<a href="/wiki/Information" title="Information">information</a> or
<a href="/wiki/Data" title="Data">data</a>, typically in rows and columns, or
possibly in a more complex structure.
```

*(Note: every tag opens and closes completely within one line)*

---

## 📤 Sample Output

```text
A table is an arrangement of
information or
data, typically in rows and columns, or
possibly in a more complex structure.
```

---

## ✅ Solution — CLI

```bash
sed 's/<[^>]*>//g'
```

## ✅ Solution — script.sed

```sed
s/<[^>]*>//g
```

```bash
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### How it works — trace for `A <b>table</b> is an arrangement of`

```
Input:   A <b>table</b> is an arrangement of

Pass 1 — s/<[^>]*>//g (first match: <b>)
         <  → opening angle bracket
         [^>]* → matches b (no > characters)
         >  → closing angle bracket
         <b> is deleted
         Remaining: A table</b> is an arrangement of

Pass 2 — still within same g scan (next match: </b>)
         <  → opening angle bracket
         [^>]* → matches /b (no > characters)
         >  → closing angle bracket
         </b> is deleted
         Remaining: A table is an arrangement of  ✓
```

---

### 🔹 The Pattern — `<[^>]*>`

```bash
s/<[^>]*>//g
```

This single pattern is the core of the solution. Breaking it down character by character:

**`<`** matches the literal opening angle bracket that begins every HTML tag.

**`[^>]*`** is a negated character class with a quantifier. The `[^>]` part matches any single character that is **not** a `>`. The `*` means zero or more of those characters. Together they match everything inside the tag — tag names, attribute names, attribute values, slashes for closing tags — as long as none of it is a `>`. This is the key insight: instead of trying to describe what is inside a tag, you simply say "everything that is not a closing `>`."

**`>`** matches the literal closing angle bracket that ends the tag.

```
<  b  >          → <b>   matched ✓
<  /  b  >       → </b>  matched ✓
<  a     h  r  e  f  =  "  .  .  .  "  >   → <a href="...">  matched ✓
<  /  a  >       → </a>  matched ✓
```

---

### 🔹 Why `[^>]*` and Not `.*`

This is the most important design decision in the pattern. Consider the alternative:

```bash
s/<.*>//g    # ← WRONG — greedy, dangerous
```

The `.*` is greedy and matches as many characters as possible, including `>`. On a line like:

```
<b>table</b> is <i>italic</i>
```

`.*` would match from the first `<` all the way to the last `>`, consuming everything in between — including the word "table", "is", and "italic". The result would be an empty line instead of `table is italic`.

`[^>]*` stops at the first `>` it encounters, so each tag is matched individually and only the tags are removed:

| Pattern | Matches on `<b>table</b>` | Result |
|---|---|---|
| `<.*>` | `<b>table</b>` (entire span) | *(empty)* — text lost! |
| `<[^>]*>` | `<b>` then `</b>` separately | `table` — correct ✓ |

---

### 🔹 Why the `/g` Flag is Essential

```bash
s/<[^>]*>//g
```

The `/g` (global) flag tells sed to keep scanning and replacing after each match rather than stopping at the first one. Without it, only the first tag on each line would be removed:

```
Without /g:
  Input:  A <b>table</b> is an arrangement of
  Output: A table</b> is an arrangement of   ✗ second tag survives

With /g:
  Input:  A <b>table</b> is an arrangement of
  Output: A table is an arrangement of        ✓ all tags removed
```

---

### ⚠️ The Multiline Tag Limitation

The solution works **only when tags open and close on the same line**. The provided sample input in the original problem contains tags that span multiple lines, such as:

```html
<a
  href="https://en.wiktionary.org/wiki/tabular#Adjective"
  class="extiw"
  title="wikt:tabular"
  >adjective</a
>
```

Since sed processes one line at a time by default, `s/<[^>]*>//g` will process each of these lines independently. On the line `<a`, it sees `<a` with no closing `>` on that line — `[^>]*` matches `a` and then the line ends, so the whole-line `<a` fragment remains. The attribute lines like `href="..."` contain no `<` or `>` so they pass through untouched as raw text. The clean output shown in the original problem statement requires **additional preprocessing** (such as joining continuation lines before tag stripping) and is **not achievable** with `s/<[^>]*>//g` alone on that specific input.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---|---|---|
| 1 | Literal match | `<` and `>` | Anchors the pattern to actual angle brackets |
| 2 | Negated character class | `[^>]` | Matches any character that is NOT a `>` |
| 3 | Zero-or-more quantifier | `*` | Matches any number of non-`>` characters (including zero, for empty tags like `<br>`) |
| 4 | Global flag | `g` | Replaces every match on the line, not just the first |
| 5 | Delete by empty replacement | `s/pattern//` | Matched text is removed — replacement string is empty |
| 6 | No `-E` needed | *(BRE)* | `[^>]*` is valid basic regex — no extended features required |

---

## ⚠️ Important Points

* **`[^>]*` stops at the first `>`** — this is what makes the pattern safe for lines with multiple tags. It never accidentally consumes text between tags
* **Empty tags are handled** — `[^>]*` uses `*` (zero or more), so tags like `<br>` or `<hr>` where nothing exists between `<` and `>` are matched and removed correctly
* **Closing tags are handled** — `</b>`, `</a>`, `</div>` all match because `/` and the tag name are simply non-`>` characters matched by `[^>]*`
* **Tags with attributes are handled** — `<a href="..." title="...">` is matched in full because all the attribute text contains no `>` characters (assuming well-formed HTML with no `>` inside attribute values)
* **Does NOT work for multiline tags** — if a tag opens on one line and its `>` appears on a later line, sed's line-by-line processing means `[^>]*` will never find the closing `>` on the first line and the tag fragment is left in the output. The problem constraint "tags open and close on the same line" is therefore a hard requirement for this solution

---

## 🧠 Memory — Pattern Matching Inside Delimiters

| Goal | Pattern | Reason |
|---|---|---|
| Match any HTML tag | `<[^>]*>` | Stop at first `>` using negated class |
| Match any tag (unsafe) | `<.*>` | Greedy — swallows text between tags |
| Remove first tag only | `s/<[^>]*>//` | No `g` flag |
| Remove all tags on line | `s/<[^>]*>//g` | With `g` flag |
| Check if line has any tag | `/<[^>]*>/` | As an address or condition |

> 💡 **Golden Rule — Matching Between Delimiters:**
> When you want to match everything **inside** a pair of delimiters (like `<` and `>`), never use greedy `.*` between them. Instead, use a **negated character class** that excludes the closing delimiter:
>
> $$\text{Pattern} = \texttt{OPEN} + \texttt{[{\char`\^}CLOSE]*} + \texttt{CLOSE}$$
>
> This guarantees each match stops at the **nearest** closing delimiter, preventing the greedy overreach that would consume text you want to keep.
>
> Examples: `<[^>]*>` for HTML tags, `"[^"]*"` for quoted strings, `([^)]*)`for parenthesised content.
