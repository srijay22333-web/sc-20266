# 📘 Student Result with Total & Serial Number (AWK)

---

## 📝 Problem Statement

You are given a CSV file where:

* First row contains subject names
* Each next row contains marks of a student

---

### 🎯 Your Task

For each student:

* Add **Serial Number (SNO)**
* Calculate **Total marks**
* Determine **Result**:

  * PASS → all marks ≥ 40
  * FAIL → any mark < 40
* Print output in **tab-separated format**

---

## 📥 Sample Input

```text id="sr1"
L1,L2,MATHS,SCIENCE,SOCIAL
86,92,78,90,85
70,80,95,88,92
92,88,35,82,78
65,20,20,85,90
88,90,92,78,85
```

---

## 📤 Sample Output

```text id="sr2"
SNO L1 L2 MATHS SCIENCE SOCIAL RESULT TOTAL
1 86 92 78 90 85 PASS 431
2 70 80 95 88 92 PASS 425
3 92 88 35 82 78 FAIL 375
4 65 20 20 85 90 FAIL 280
5 88 90 92 78 85 PASS 433
```

---

## 💻 Solution (script.awk)

```awk id="sr3"
#!/usr/bin/gawk -f

BEGIN {
    FS=","; OFS="\t"
}

NR == 1 {
    print "SNO", $1, $2, $3, $4, $5, "RESULT", "TOTAL"
    next
}

{
    total = 0
    result = "PASS"

    for (i = 1; i <= NF; i++) {
        total += $i
        if ($i < 40)
            result = "FAIL"
    }

    print NR-1, $1, $2, $3, $4, $5, result, total
}
```

---

## 🔍 Explanation (Simple)

---

### 🔹 Header Handling

```awk id="sr4"
NR == 1
```

👉 Prints new header with:

* SNO
* RESULT
* TOTAL

---

### 🔹 Loop Through Marks

```awk id="sr5"
for (i=1; i<=NF; i++)
```

👉 Adds marks
👉 Checks fail condition

---

### 🔹 Result Logic

```awk id="sr6"
if ($i < 40)
    result = "FAIL"
```

👉 If any subject < 40 → FAIL

---

### 🔹 Total

```awk id="sr7"
total += $i
```

👉 Sum of all subjects

---

### 🔹 Serial Number

```awk id="sr8"
NR-1
```

👉 Skips header

---

## 🎯 Key Idea

👉
**Loop → Sum → Condition → Print**

---

## ⚠️ Important Points

* Use `OFS="\t"` → tab output
* Use `next` → skip header processing
* Result depends on any subject

---

## 🧠 Memory Trick

👉
**Any <40 → FAIL
Else → PASS**

---

## 🚀 Final Insight

This is a **combo problem**:

* loop
* sum
* condition
* formatting

---
