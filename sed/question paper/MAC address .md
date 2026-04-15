# MAC Address Regex

---

## 📝 Problem Statement

Match MAC address format

---

## ✅ Solution (ERE)

```regex
[0-9A-Fa-f]{2}(:[0-9A-Fa-f]{2}){5}
```

---

## 🔍 Explanation

* 6 groups of hex digits
* separated by `:`

---

---

# Print Lines 4 to 7

---

## 📝 Problem Statement

Print lines from 4 to 7

---

## 📥 Sample Input

```text
line1
line2
line3
line4
line5
line6
line7
line8
```

---

## 📤 Sample Output

```text
line4
line5
line6
line7
```

---

## ✅ Solution — CLI

```bash
sed -n '4,7p' myfile
```

---

## 🔍 Explanation

* `-n` → suppress default
* `4,7p` → print range

---
