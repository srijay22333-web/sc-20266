# Swap Two Words using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Swaps two words in each line

---

## 📥 Sample Input

```text
hello world
```

---

## 📤 Sample Output

```text
world hello
```

---

## ✅ Solution — CLI

```bash
sed -E 's/([a-zA-Z]+) ([a-zA-Z]+)/\2 \1/' phrases.txt
```

---

## ✅ Solution — script.sed

```sed
s/([a-zA-Z]+) ([a-zA-Z]+)/\2 \1/
```

```bash
sed -E -f script.sed phrases.txt
```

---

## 🔍 Explanation

* `([a-zA-Z]+)` → capture words
* `\2 \1` → swap

---
---
