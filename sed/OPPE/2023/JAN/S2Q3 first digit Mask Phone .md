# Mask Phone Numbers in Text using sed (ERE)

---

## 📝 Problem Statement

Write a **sed script** that:

* Masks the **first 6 digits** of a 10-digit phone number
* Keeps only the **last 4 digits visible**
* The phone number:

  * Can appear **anywhere in the text**
  * Must be exactly **10 digits**
  * Must start with **6, 7, 8, or 9**
* Other numbers should remain unchanged

---

## 📥 Sample Input

```text id="ph21"
Yesterday I received a message from 9876543210, it has numerical sequence 1234567890 in it. then soon after I received the same message from 9876543211 as well. Then I called 9876543212 to inquire about it, they gave a reference id AHDISJ12354 and a fallback code 3245112341 as well.
```

---

## 📤 Sample Output

```text id="ph22"
Yesterday I received a message from ******3210, it has numerical sequence 1234567890 in it. then soon after I received the same message from ******3211 as well. Then I called ******3212 to inquire about it, they gave a reference id AHDISJ12354 and a fallback code 3245112341 as well.
```

---

## 💻 Solution (Command Line - ERE)

```bash id="ph23"
sed -E 's/\b[6789][0-9]{5}([0-9]{4})\b/******\1/g'
```

---

## 💻 Solution (script.sed - ERE)

```sed id="ph24"
s/\b[6789][0-9]{5}([0-9]{4})\b/******\1/g
```

Run using:

```bash id="ph25"
sed -E -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Match Valid Phone Number

```bash id="ph26"
\b[6789][0-9]{5}([0-9]{4})\b
```

* `\b` → word boundary
* `[6789]` → first digit must be 6, 7, 8, or 9
* `[0-9]{5}` → next 5 digits
* `([0-9]{4})` → last 4 digits (captured)
* `\b` → ensures exactly 10 digits

---

### 🔹 Replace First 6 Digits

```bash id="ph27"
******\1
```

* Masks first 6 digits
* Keeps last 4 digits using `\1`

---

### 🔹 Global Replacement

```bash id="ph28"
g
```

* Replaces all occurrences in the line

---

## 🎯 Key Idea

👉 Match full number → capture last 4 → replace prefix

---

## ⚠️ Important Points

* Use `-E` for `{}` and `()`
* `\b` ensures exact word match
* Only valid phone numbers are masked

---

## 🧠 Memory Trick

👉 **Start digit check → 6 digits → keep last 4 → replace rest**
