# Match Price Format using Regex

---

## 📝 Problem Statement

Write a **regex** that matches:

* `$3.45`, `$23.32`, `$400`

---

## ✅ Solution (ERE)

```regex
^\$[0-9]+(\.[0-9]{2})?$
```

---

## 🔍 Explanation

* `\$` → literal `$`
* `[0-9]+` → digits
* `(\.[0-9]{2})?` → optional decimal

---

---

# Swap First Two CSV Fields

---

## 📝 Problem Statement

Write a **sed script** that swaps first two fields in CSV

---

## 📥 Sample Input

```text
A,B,C
1,2,3
```

---

## 📤 Sample Output

```text
B,A,C
2,1,3
```

---

## ✅ Solution — CLI (BRE)

```bash
sed 's/^\([^,]*\),\([^,]*\),\(.*\)/\2,\1,\3/' file.csv
```

---

## ✅ Solution — script.sed

```sed
s/^\([^,]*\),\([^,]*\),\(.*\)/\2,\1,\3/
```

---

## 🔍 Explanation

* `\([^,]*\)` → capture fields
* `\2,\1,\3` → swap

---
