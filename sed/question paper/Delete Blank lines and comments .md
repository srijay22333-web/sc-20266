# Delete Blank and Comment Lines

---

## 📝 Problem Statement

Write a **sed script** that:

* Removes:

  * Blank lines
  * Lines starting with `#`

---

## 📥 Sample Input

```text
# comment

hello

# test
world
```

---

## 📤 Sample Output

```text
hello
world
```

---

## ✅ Solution — CLI

```bash
sed '/^\s*$/d;/^\s*#/d' file.conf
```

---

## ✅ Solution — script.sed

```sed
/^\s*$/d
/^\s*#/d
```

```bash
sed -f script.sed file.conf
```

---

## 🔍 Explanation

* `^\s*$` → empty line
* `^\s*#` → comment line
* `d` → delete

---
