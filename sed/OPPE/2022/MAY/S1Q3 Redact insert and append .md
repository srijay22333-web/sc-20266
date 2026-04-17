# Redact Sensitive Data using sed

---

## 📝 Problem Statement

Write a **sed script (`script.sed`)** that:

* Replaces entire line with `##REDACTED##` if:

  * Line contains:

    * `Password` / `password`
    * `Address` / `address`
  * OR line contains **only digits**
* Adds:

  * `##CONFIDENTIAL##` at the **beginning** of file
  * `##CONFIDENTIAL##` at the **end** of file

---

## 📥 Sample Input

```text id="red1"
This is another proctected file
this contain one protected info
as below
9897623637
and it needs to be redacted.
another phone number
9893213738
but not below ones
91 982382378
928923bd923
```

---

## 📤 Sample Output

```text id="red2"
##CONFIDENTIAL##
This is another proctected file
this contain one protected info
as below
##REDACTED##
and it needs to be redacted.
another phone number
##REDACTED##
but not below ones
91 982382378
928923bd923
##CONFIDENTIAL##
```

---

## ✅ Solution — CLI (ERE)

```bash id="red3"
sed -E '
1i ##CONFIDENTIAL##
$a ##CONFIDENTIAL##

/[pP]assword|[aA]ddress/c##REDACTED##
/^[0-9]+$/c##REDACTED##
' input.txt
```

---

## ✅ Solution — script.sed

```sed id="red4"
1i ##CONFIDENTIAL##
$a ##CONFIDENTIAL##

/[pP]assword|[aA]ddress/c##REDACTED##
/^[0-9]+$/c##REDACTED##
```

Run using:

```bash id="red5"
sed -E -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Add Header and Footer

```bash id="red6"
1i ##CONFIDENTIAL##
$a ##CONFIDENTIAL##
```

* `1i` → insert before first line
* `$a` → append after last line

---

### 🔹 Replace Sensitive Words

```bash id="red7"
/[pP]assword|[aA]ddress/
```

* Matches both uppercase/lowercase variants

---

### 🔹 Replace Entire Line

```bash id="red8"
c##REDACTED##
```

* Replaces whole line

---

### 🔹 Match Only Digits

```bash id="red9"
^[0-9]+$
```

* Ensures line contains only numbers

---

## 🎯 Key Idea

👉 Detect sensitive → replace full line → add boundaries

---

## ⚠️ Important Notes (Your Answer Review)

✔️ Your logic: **Correct**
✔️ Patterns: **Correct**
✔️ Commands: **Correct**

⚠️ Only improvement:

* Must run with `-E` for `+` and `|`
* Otherwise use BRE (`\+`, `\|`)

---

## 🧠 Memory Trick

👉 **Sensitive → REDACTED, File → CONFIDENTIAL borders**

---
