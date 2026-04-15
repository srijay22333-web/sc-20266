# Skip First 3 Lines

---

## 📝 Problem Statement

Print all lines except first 3

---

## 📥 Sample Input

```text
line1
line2
line3
line4
line5
```

---

## 📤 Sample Output

```text
line4
line5
```

---

## ✅ Solution — CLI

```bash
sed '1,3d' data.txt
```

---

## ✅ Alternative

```bash
sed -n '4,$p' data.txt
```

---

## 🔍 Explanation

* `1,3d` → delete first 3
* `4,$p` → print from line 4

---
