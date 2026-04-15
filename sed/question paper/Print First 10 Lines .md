# Print First 10 Lines

---

## 📝 Problem Statement

Write a **sed command** that:

* Prints first 10 lines

---

## 📥 Sample Input

```text
(line1)
(line2)
...
(lineN)
```

---

## 📤 Sample Output

```text
first 10 lines only
```

---

## ✅ Solution — CLI

```bash
sed -n '1,10p' file.txt
```

---

## ✅ Alternative Solutions

```bash
sed 11q file.txt
```

```bash
awk 'NR<=10' file.txt
```

---

## 🔍 Explanation

* `1,10p` → print lines 1 to 10
* `-n` → suppress default output
* `11q` → quit after line 10

---
