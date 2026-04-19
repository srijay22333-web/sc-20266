# 📘 Add Total Energy Column (CSV)

---

## 📝 Problem Statement

A CSV file contains three columns:

```text id="csv_cols"
time, energy1, energy2
```

👉 Create a new column **`totalenergy`** which is the sum of `energy1` and `energy2`.

---

## 🎯 Task

* Read CSV input correctly
* Add a new column header `totalenergy`
* Compute sum of `energy1 + energy2` for each row
* Print updated CSV

---

## 📥 Sample Input

```text id="csv_in"
time,energy1,energy2
0,1001,-1500
10,1005,-1650
15,1007,-1734
```

---

## 📤 Sample Output

```text id="csv_out"
time,energy1,energy2,totalenergy
0,1001,-1500,-499
10,1005,-1650,-645
15,1007,-1734,-727
```

---

## ⚠️ Important Note

* `FS=","` → sets input as CSV
* `OFS=","` → ensures output is also CSV
* `$2 + $3` → performs numeric addition
* `$(NF+1)` → appends a new column

---

## ✅ Solution — script.awk

```awk id="awk_csv_total"
BEGIN {
    FS = ","
    OFS = ","
}

NR == 1 {
    $(NF+1) = "totalenergy"
}

NR > 1 {
    $(NF+1) = $2 + $3
}

{
    print $0
}
```

---

## ▶️ Run

```bash id="run_csv"
awk -f script.awk input.csv
```

---

## 🔍 Explanation

* `BEGIN` → initializes CSV handling
* `NR == 1` → header row → add column name
* `NR > 1` → data rows → compute sum
* `$(NF+1)` → adds new field at the end
* `print $0` → prints full updated row

---

## 🧠 Visual Understanding

### ▶ Header Update

```text id="csv_header"
time,energy1,energy2
→
time,energy1,energy2,totalenergy
```

---

### ▶ Row Transformation

```text id="csv_calc"
1001 + (-1500) = -499
1005 + (-1650) = -645
1007 + (-1734) = -727
```

---

## 🔥 Key Concept

```text id="csv_key"
$(NF+1) → append new column dynamically
```

---

