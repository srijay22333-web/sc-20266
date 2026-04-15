# Replace 2nd Occurrence using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Replaces only the **second occurrence** of `"apple"` with `"orange"`
* Processes each line independently

---

## 📥 Sample Input

```text
apple banana apple grape apple
apple apple orange apple
banana apple apple
```

---

## 📤 Sample Output

```text
apple banana orange grape apple
apple orange orange apple
banana apple orange
```

---

## ✅ Solution — CLI

```bash
sed 's/apple/orange/2' data.txt
```

---

## ✅ Solution — script.sed

```sed
s/apple/orange/2
```

```bash
sed -f script.sed data.txt
```

---

## 🔍 Explanation

* `s/apple/orange/2` → replaces only **2nd occurrence per line**
* Not global (`g`) → only specific occurrence

---
