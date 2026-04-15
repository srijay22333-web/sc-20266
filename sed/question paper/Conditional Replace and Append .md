# Conditional Replace and Append using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Selects lines starting with `error`
* Replaces `error` with `warning`
* Appends `" - needs review"` at the end

---

## 📥 Sample Input

```text
error disk failure
info system running
error memory leak
```

---

## 📤 Sample Output

```text
warning disk failure - needs review
info system running
warning memory leak - needs review
```

---

## ✅ Solution — CLI

```bash
sed '/^error/{
s/^error/warning/
s/$/ - needs review/
}' file.txt
```

---

## ✅ Solution — script.sed

```sed
/^error/{
s/^error/warning/
s/$/ - needs review/
}
```

```bash
sed -f script.sed file.txt
```

---

## 🔍 Explanation

* `/^error/` → match lines starting with `error`
* `s/^error/warning/` → replace at beginning
* `s/$/.../` → append at end

---
