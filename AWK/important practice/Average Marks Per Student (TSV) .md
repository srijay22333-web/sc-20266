# 📘 Average Marks Per Student (TSV)

---

## 📝 Problem Statement

Given a **TSV (Tab-Separated Values)** file containing student records:

👉 Compute the **average marks** for each student.

---

## 🎯 Task

* Read TSV input
* Extract marks from columns
* Calculate average of marks
* Print student ID, name, and average (2 decimal places)

---

## 📥 Sample Input

```text id="avg_in"
1	Bob	80	75	90	245
2	Alice	88	92	85	265
3	Charlie	78	82	79	239
```

---

## 📤 Sample Output

```text id="avg_out"
1 Bob 81.67
2 Alice 88.33
3 Charlie 79.33
```

---

## ⚠️ Important Note

* `FS = "\t"` → sets input format to **tab-separated**
* Marks are in columns `$3`, `$4`, `$5`
* Last column (total) is ignored
* `printf` ensures **formatted output (2 decimal places)**

---

## ✅ Solution — average_marks.awk

```awk id="awk_avg_marks"
BEGIN {
    FS = "\t"
}

{
    avg = ($3 + $4 + $5) / 3
    printf "%s %s %.2f\n", $1, $2, avg
}
```

---

## ▶️ Run

```bash id="run_avg"
awk -f average_marks.awk input.tsv
```

---

## 🔍 Explanation

* `FS="\t"` → splits fields using tab
* `$1` → student ID
* `$2` → student name
* `$3,$4,$5` → marks
* `avg = (...) / 3` → computes average
* `printf "%.2f"` → formats to 2 decimal places

---

## 🧠 Visual Understanding

### ▶ Data Extraction

```text id="avg_extract"
Bob → 80, 75, 90
Alice → 88, 92, 85
```

---

### ▶ Calculation

```text id="avg_calc"
(80 + 75 + 90) / 3 = 81.67
(88 + 92 + 85) / 3 = 88.33
```

---

## 🔥 Key Concept

```text id="avg_key"
printf "%.2f" → control decimal precision
```

---

## ✅ Alternative (Dynamic Columns)

```awk id="avg_alt"
BEGIN { FS="\t" }

{
    sum = 0
    for (i = 3; i <= NF-1; i++) {
        sum += $i
    }
    avg = sum / (NF - 3)
    printf "%s %s %.2f\n", $1, $2, avg
}
```

---

## ▶️ Run

```bash id="run_avg_alt"
awk -f average_marks.awk input.tsv
```
