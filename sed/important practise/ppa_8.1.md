## Swap Case and Words using SED

---

# Problem Statement

You are given a file `words.txt` where each line contains a string in the format:

```text
FIRST_second
```

Where:

* `FIRST` → all **uppercase letters**
* `second` → all **lowercase letters**

---

### 🎯 Task

Convert each string into:

```text
SECOND_first
```

After conversion:

* Swap the two words
* Convert:

  * uppercase → lowercase
  * lowercase → uppercase

---

# Input Format

```text
Each line: FIRST_second
```

---

## Sample Input

```text
AD_ux
AD_uy
```

---

## Sample Output

```text
UX_ad
UY_ad
```

---

# Solution

```bash
sed -E 's/([A-Z]+)_([a-z]+)/\U\2_\L\1/' words.txt
```

---

# Explanation

### 🔹 Pattern Matching

```sed
([A-Z]+)_([a-z]+)
```

* `([A-Z]+)` → captures uppercase word (`FIRST`)
* `_` → matches underscore
* `([a-z]+)` → captures lowercase word (`second`)

---

### 🔹 Replacement

```sed
\U\2_\L\1
```

* `\2` → second word
* `\1` → first word

👉 Case conversion:

* `\U` → convert to **uppercase**
* `\L` → convert to **lowercase**

---

### 🔁 Transformation

```text
AD_ux
↓
UX_ad
```

---

## 🧠 Key Concepts

| Feature    | Meaning                |
| ---------- | ---------------------- |
| `-E`       | enables extended regex |
| `()`       | capture groups         |
| `\1`, `\2` | reuse captured parts   |
| `\U`       | uppercase conversion   |
| `\L`       | lowercase conversion   |

---

## ⚠️ Important Note

👉 `\U` and `\L` are supported in **GNU sed**
👉 May not work in older sed versions

---

👉 Run:

```bash
bash swapcase.sh
```

or directly:

```bash
sed -E 's/([A-Z]+)_([a-z]+)/\U\2_\L\1/' words.txt
```

---

