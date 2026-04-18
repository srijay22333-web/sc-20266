# 📘 Total Marks Per Student (Sorted)

---

## 📝 Problem Statement

You are given a file `marks.txt` where each line contains:

```
Name Subject Marks
```

---

## 🎯 Task

* Compute total marks per student
* Sort results in **descending order of marks**

---

## 📥 Sample Input

```text
Alice Math 80
Bob Physics 70
Alice Physics 90
Bob Math 60
Charlie Math 75
Charlie Physics 85
```

---

## 📤 Sample Output

```text
Alice 170
Charlie 160
Bob 130
```

---

## ✅ Solution 1 — script.awk (Sorting inside AWK)

```awk
BEGIN {
    FS = " "
}

{
    subject[$1] += $3
}

END {
    n = asorti(subject, sorted, "@val_num_desc")

    for (i = 1; i <= n; i++) {
        name = sorted[i]
        print name, subject[name]
    }
}
```

### ▶️ Run

```bash
awk -f script.awk marks.txt
```

---

## 🔍 Explanation

* `FS = " "` → split fields by space
* `subject[$1] += $3` → accumulate marks per student
* `asorti(... "@val_num_desc")` → sort by marks (descending)
* `sorted[i]` → gives names in sorted order
* loop → prints final result

---

## ✅ Solution 2 — script.awk + sort

```awk
BEGIN {
    FS = " "
}

{
    subject[$1] += $3
}

END {
    for (name in subject) {
        print name, subject[name]
    }
}
```

---

### ▶️ Run with sorting

```bash
awk -f script.awk marks.txt | sort -k2 -nr
```

---

## 🔍 Explanation

* AWK computes totals
* `sort -k2 -nr`:

  * `-k2` → sort by marks column
  * `-n` → numeric sort
  * `-r` → descending order

---

## 🧠 Key Concept

```text
Read → Accumulate → Store → Sort → Print
```

