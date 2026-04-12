# Mask Phone Numbers using sed (ERE)

---

## 📝 Problem Statement

Write a **sed script** that:

* Masks the **first 6 digits** of a 10-digit phone number
* Replaces them with `******`
* Keeps only the **last 4 digits visible**

---

## 📥 Sample Input

```text id="ph11"
9876543210
1234567890
5556667777
```

---

## 📤 Sample Output

```text id="ph12"
******3210
******7890
******7777
```

---

## 💻 Solution 1 (Using Capture Group)

```bash id="ph13"
sed -E 's/^[0-9]{6}([0-9]{4})/******\1/'
```

---

## 💻 Solution 2 (Simple Replacement)

```bash id="ph14"
sed -E 's/^[0-9]{6}/******/'
```

---

## 🔍 Explanation

### 🔹 Solution 1

```bash id="ph15"
^[0-9]{6}([0-9]{4})
```

* `^` → start of line
* `[0-9]{6}` → first 6 digits
* `([0-9]{4})` → last 4 digits (captured as `\1`)

👉 Replacement:

```bash id="ph16"
******\1
```

* Masks first 6 digits
* Keeps last 4 digits

---

### 🔹 Solution 2

```bash id="ph17"
^[0-9]{6}
```

* Matches first 6 digits

👉 Replacement:

```bash id="ph18"
******
```

* Replaces only first 6 digits
* Remaining digits automatically stay

---

## 🎯 Key Idea

👉 Match from **start of line** using ERE (`{}` without backslashes)

---

## ⚠️ Important Points

* Use `-E` for extended regex
* No need for `g` (only one match at start)
* Works for fixed 10-digit format

---

## 🧠 Memory Trick

👉 **ERE → no slashes → `{6}` → replace first part**
