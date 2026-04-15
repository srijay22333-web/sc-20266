# Structured Pattern Match (ERE)

---

## 📝 Problem Statement

Match lines with:

1. lowercase word
2. 2–3 digit number
3. lowercase word
4. exactly 3-digit number

---

## ✅ Solution

```regex
^[a-z]+\s+[0-9]{2,3}\s+[a-z]+\s+[0-9]{3}$
```

---

## 🔍 Explanation

* `[a-z]+` → word
* `[0-9]{2,3}` → 2–3 digits
* `[0-9]{3}` → exactly 3 digits

---
