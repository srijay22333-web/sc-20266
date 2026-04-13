# Visualize Tabs and Line Endings using sed

---

## 📝 Problem Statement

A student wants to visualize hidden characters in a file.

Write a **sed script** that:

* Replaces every **tab character** with `^I`
* Marks the **end of each line** with `$`

---

## 📥 Sample Input

```text id="vis1"
Hello	World
This	is	a	test
Line without tab
```

*(Note: tabs are present between words)*

---

## 📤 Sample Output

```text id="vis2"
Hello^IWorld$
This^Iis^Ia^Itest$
Line without tab$
```

---

## 💻 Solution (Command Line)

```bash id="vis3"
sed -E 's/\t/^I/g; s/$/$/'
```

---

## 💻 Solution (script.sed)

```sed id="vis4"
s/\t/^I/g
s/$/$/
```

Run using:

```bash id="vis5"
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Replace Tab

```bash id="vis6"
\t
```

* Matches tab character

```bash id="vis7"
^I
```

* Replaces tab with visible `^I`

---

### 🔹 Mark End of Line

```bash id="vis8"
$
```

* Matches end of line

```bash id="vis9"
s/$/$/
```

* Appends `$` at end

---

## 🎯 Key Idea

👉 Replace hidden characters with visible symbols

---

## ⚠️ Important Points

* `\t` represents tab
* `$` matches end of line (not a character)
* `g` ensures all tabs are replaced

---

## 🧠 Memory Trick

👉 **Tab → ^I, End → $**
