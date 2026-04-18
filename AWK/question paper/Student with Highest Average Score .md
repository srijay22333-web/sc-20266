# 📘 Student with Highest Average Score

---

## 📝 Problem Statement

Given a file where each line contains a student name followed by marks, find the student with the **highest average score**.

---

## 🎯 Task

* Compute average marks per student
* Identify the student with the highest average
* Print the student name and average

---

## 📥 Sample Input

```text id="3g4c0m"
John 80 90 70
Mary 85 75 95
Alex 60 70 65
```

---

## 📤 Sample Output

```text id="b9q8t2"
Mary 85
```

---

## ✅ Solution — script.awk

```awk id="6t6j2q"
BEGIN {
    FS = " "
}

{
    sum = 0

    for (i = 2; i <= NF; i++) {
        sum += $i
    }

    avg = sum / (NF - 1)

    if (avg > max) {
        max = avg
        topper = $1
    }
}

END {
    print topper, max
}
```

---

## ▶️ Run

```bash id="h7k3dx"
awk -f script.awk marks.txt
```

---

## 🔍 Explanation

* `FS = " "` → splits fields by space
* `for (i = 2; i <= NF; i++)` → loops through marks only
* `sum += $i` → adds all marks
* `avg = sum / (NF - 1)` → calculates average
* `if (avg > max)` → tracks highest average
* `topper = $1` → stores student name
* `print topper, max` → prints result

---

## 🧠 Visual Understanding

### ▶ John

```text id="k3y7s1"
Marks: 80 90 70
Sum = 240
Avg = 80
```

---

### ▶ Mary

```text id="n2c5v8"
Marks: 85 75 95
Sum = 255
Avg = 85  ← highest
```

---

### ▶ Alex

```text id="f8m1zp"
Marks: 60 70 65
Sum = 195
Avg = 65
```

---

## 🔥 Key Concept

```text id="z4u1x9"
Loop → Sum → Average → Track Maximum
```

---
