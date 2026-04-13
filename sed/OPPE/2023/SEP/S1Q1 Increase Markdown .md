# Increase Markdown Heading Level using sed

---

## 📝 Problem Statement

In a markdown file, headings are defined using `#` at the beginning of a line:

* `#` → level 1
* `##` → level 2
* `###` → level 3

Write a **sed script** that:

* Increases the heading level by **1** (adds one `#` at the beginning)
* Applies only to lines that start with `#`
* Adds the line **`# Text editors in Linux`** at the beginning of the file

---

## 📥 Sample Input

```text id="md1"
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

```text id="md2"
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

## 💻 Solution (Command Line)

```bash id="md3"
sed '1i # Text editors in Linux
/^#/ s/^/#/' 
```

---

## 💻 Solution (script.sed)

```sed id="md4"
1i # Text editors in Linux
/^#/ s/^/#/
```

Run using:

```bash id="md5"
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Insert First Line

```bash id="md6"
1i # Text editors in Linux
```

* Adds the heading at the beginning of the file

---

### 🔹 Match Heading Lines

```bash id="md7"
/^#/
```

* Selects lines starting with `#`

---

### 🔹 Increase Heading Level

```bash id="md8"
s/^/#/
```

* Adds one more `#` at the beginning
* Converts:

  * `#` → `##`
  * `##` → `###`

---

## 🎯 Key Idea

👉 Match heading lines → add one `#` at start

---

## ⚠️ Important Points

* `^` ensures modification only at line start
* Non-heading lines remain unchanged
* Order matters: insert first, then modify

---

## 🧠 Memory Trick

👉 **If line starts with # → add one more #**

