# 📘 Print First 10 Lines

---

## 📝 Problem Statement

Print the **first 10 lines** of a file.

---

## 🎯 Task

* Read input line by line
* Output only the first **10 lines**

---

## 📥 Sample Input

```text id="first10_in"
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
Line 11
Line 12
```

---

## 📤 Sample Output

```text id="first10_out"
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

---

## ⚠️ Important Note

* `NR` is a built-in AWK variable
* It represents the **current line number** (Number of Records)

---

## ✅ Solution — script.awk

```awk id="awk_first10"
NR <= 10
```

---

## ▶️ Run

```bash id="run_first10"
awk -f script.awk input.txt
```

---

## 🔍 Explanation

* `NR <= 10` → condition is true for the first 10 lines
* AWK prints the line automatically when the condition is true
* After line 10, condition becomes false → no output

---

## 🧠 Visual Understanding

### ▶ Line Processing

```text id="line_process"
NR=1  → print
NR=2  → print
...
NR=10 → print
NR=11 → skip
NR=12 → skip
```

---

## 🔥 Key Concept

```text id="key_first10"
NR → current record (line) number
```

---

## ✅ Alternative (Explicit Print)

```awk id="alt_first10"
NR <= 10 {
    print
}
```

---
