# Remove HTML Tags using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Removes all **HTML tags** from the input
* Keeps only the **text content**
* Assume that:

  * Tags open and close on the **same line**

---

## 📥 Sample Input

```text id="html1"
A <b>table</b> is an arrangement of
<a href="/wiki/Information" title="Information">information</a> or
<a href="/wiki/Data" title="Data">data</a>, typically in rows and columns, or
possibly in a more complex structure. Tables are widely used in
<a href="/wiki/Communication" title="Communication">communication</a>,
<a href="/wiki/Research" title="Research">research</a>, and
<a href="/wiki/Data_analysis" title="Data analysis">data analysis</a>. Tables
appear in print media, handwritten notes, computer software, architectural
ornamentation, traffic signs, and many other places. The precise conventions and
terminology for describing tables vary depending on the context. Further, tables
differ significantly in variety, structure, flexibility, notation,
representation and use.<sup id="cite_ref-Fink000_1-0" class="reference"
  ><a href="#cite_note-Fink000-1">[1]</a></sup
><sup id="cite_ref-Mcnabb000_2-0" class="reference"
  ><a href="#cite_note-Mcnabb000-2">[2]</a></sup
><sup id="cite_ref-Morgan000_3-0" class="reference"
  ><a href="#cite_note-Morgan000-3">[3]</a></sup
><sup id="cite_ref-Robey000_4-0" class="reference"
  ><a href="#cite_note-Robey000-4">[4]</a></sup
><sup id="cite_ref-Zielinski000_5-0" class="reference"
  ><a href="#cite_note-Zielinski000-5">[5]</a></sup
>
Information or data conveyed in table form is said to be in
<b>tabular</b> format (<a
  href="https://en.wiktionary.org/wiki/tabular#Adjective"
  class="extiw"
  title="wikt:tabular"
  >adjective</a
>). In books and technical articles, tables are typically presented apart from
the main text in numbered and captioned
<a href="/wiki/Floating_block" class="mw-redirect" title="Floating block"
  >floating blocks</a
>.
```

---

## 📤 Sample Output

```text id="html2"
A table is an arrangement of information or data, typically in rows and columns, or possibly in a more complex structure. Tables are widely used in communication, research, and data analysis. Tables appear in print media, handwritten notes, computer software, architectural ornamentation, traffic signs, and many other places. The precise conventions and terminology for describing tables vary depending on the context. Further, tables differ significantly in variety, structure, flexibility, notation, representation and use.[1][2][3][4][5] Information or data conveyed in table form is said to be in tabular format (adjective). In books and technical articles, tables are typically presented apart from the main text in numbered and captioned floating blocks.
```

---

## 💻 Solution (Command Line)

```bash id="html3"
sed -E 's/<[^>]*>//g'
```

---

## 💻 Solution (script.sed)

```sed id="html4"
s/<[^>]*>//g
```

Run using:

```bash id="html5"
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Match HTML Tags

```bash id="html6"
<[^>]*>
```

* `<` → start of tag
* `[^>]*` → any characters except `>`
* `>` → end of tag

👉 Matches:

* `<b>`
* `</a>`
* `<a href="...">`

---

### 🔹 Remove Tags

```bash id="html7"
s/<[^>]*>//g
```

* Replaces tags with nothing
* `g` → removes all tags in the line

---

## 🎯 Key Idea

👉 Match `<...>` → remove → keep text

---

## ⚠️ Important Points

* Works when tags are on the **same line**
* Does not handle nested multi-line tags
* `g` is required for multiple tags

---

## 🧠 Memory Trick

👉 **< tag > → remove everything inside → keep text**
