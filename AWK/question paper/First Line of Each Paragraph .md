# 📘 First Line of Each Paragraph

---

## 📝 Problem Statement

Given a file containing multiple paragraphs separated by blank lines, print:

👉 The **first line of each paragraph**

---

## 🎯 Task

* Treat each paragraph as a record
* Extract and print the **first line** from each paragraph

---

## 📥 Sample Input

```text id="para_in"
This is paragraph one.
It has multiple lines.

This is paragraph two.
Another line here.

Third paragraph starts here.
And continues.
```

---

## 📤 Sample Output

```text id="para_out"
This is paragraph one.
This is paragraph two.
Third paragraph starts here.
```

---

## ⚠️ Important Note

* Default AWK behavior reads input **line by line**
* We override this using:

  * `RS = ""` → makes a **blank line** the record separator
  * `FS = "\n"` → splits paragraph into lines

---

## ✅ Solution — script.awk

```awk id="awk_para_firstline"
BEGIN {
    RS = ""
    FS = "\n"
}
{
    print $1
}
```

---

## ▶️ Run

```bash id="run_para"
awk -f script.awk input.txt
```

---

## 🔍 Explanation

* `RS = ""` → groups text into paragraphs
* `FS = "\n"` → splits each paragraph into individual lines
* `$1` → refers to the **first line of the paragraph**
* `print $1` → outputs the required result

---

## 🧠 Visual Understanding

### ▶ Paragraph Splitting

```text id="para_split"
Record 1:
Line 1
Line 2

Record 2:
Line 1
Line 2
```

---

### ▶ Extraction

```text id="para_extract"
Record 1 → Line 1
Record 2 → Line 1
Record 3 → Line 1
```

---

## 🔥 Key Concept

```text id="key_para"
RS = ""  → paragraph mode in AWK
```

---
