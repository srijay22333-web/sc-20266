# 📘 Student Attendance Percentage using AWK

---

## 📝 Problem Statement

You are given a CSV file where:

* First row contains:

  * `Date` + student names
* Each next row contains:

  * Date
  * `1` (present) or `0` (absent) for each student

---

### 🎯 Your Task

For each student:

* Count number of **present days**
* Compute **percentage attendance**
* Print:

```text id="att1"
StudentName Percentage
```

---

## 📥 Sample Input

```text id="att2"
Date,Ajay,Neha,Mayuri,Gaurav,Nitish,Minami
20/04/2023,1,1,0,1,0,1
21/04/2023,1,1,1,1,1,1
22/04/2023,0,1,0,1,1,1
23/04/2023,1,0,1,0,1,1
```

---

## 📤 Sample Output

```text id="att3"
Ajay 75
Neha 75
Mayuri 50
Gaurav 75
Nitish 75
Minami 100
```

---

## 💻 Solution (script.awk)

```awk id="att4"
#!/usr/bin/gawk -f

BEGIN {
    FS = ","
}

NR == 1 {
    col_count = NF

    for (i = 2; i <= NF; i++) {
        name[i] = $i        # store student names
    }
}

NR > 1 {
    total_days++

    for (i = 2; i <= NF; i++) {
        present[i] += $i    # count attendance
    }
}

END {
    for (i = 2; i <= col_count; i++) {
        percent = (present[i] / total_days) * 100
        print name[i], percent
    }
}
```

---

## 🔍 Explanation

---

### 🔹 Store Student Names

```awk id="att5"
NR == 1
```

👉 First row → header
👉 Save names using column index

---

### 🔹 Count Total Days

```awk id="att6"
total_days++
```

👉 Counts number of rows (excluding header)

---

### 🔹 Count Present Days

```awk id="att7"
present[i] += $i
```

👉 Adds `1` for present

---

### 🔹 Calculate Percentage

```awk id="att8"
(present[i] / total_days) * 100
```

👉 Attendance percentage

---

### 🔹 Print Output

```awk id="att9"
print name[i], percent
```

👉 Student name + percentage

---

## 🎯 Key Idea

👉 Use column index as key:

```awk id="att10"
name[i], present[i]
```

---

## ⚠️ Important Points

* Skip first column (Date)
* Use `NR==1` for header
* Use arrays for column tracking

---

## 🧠 Memory Trick

👉
**Header → store names**
**Rows → count 1’s**
**END → calculate %**

---

## 🚀 Final Insight

This problem teaches:

* Column-wise processing
* Arrays with index
* Aggregation

---
