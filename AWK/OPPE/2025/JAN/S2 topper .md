
# 📘 Find Topper (Highest Average) using AWK

---

## 📝 Problem Statement

You are given a **TSV file** where:

* First row = header
* Next rows = student marks

```
Name   Math   Science   English
```

---

### 🎯 Your Task

* Skip header
* Compute **average marks** for each student
* Find student with **highest average**
* Print:

```text
Topper: <Name>
```

---

## 📥 Sample Input

```text
Name	Math	Science	English
Maya	80	85	78
Shreya	90	92	88
Ananya	75	80	70
Olivia	88	86	84
Michael	82	79	85
```

---

## 📤 Sample Output

```text
Topper: Shreya
```

---

# ✅ Solution (Clean Version)

```awk
#!/usr/bin/gawk -f

BEGIN {
    FS = "\t"
    max = -1   # important initialization
}

NR == 1 {
    next
}

{
    avg = ($2 + $3 + $4) / 3

    if (avg > max) {
        max = avg
        topper = $1
    }
}

END {
    print "Topper:", topper
}
```

---

# 🔍 What You Did Right ✅

✔ Used `FS="\t"`
✔ Skipped header
✔ Calculated average
✔ Used max logic

---

# ⚠️ Small Improvement (Important)

## ❗ Initialize max

```awk
max = -1
```

👉 Why?

* If all averages are **0 or negative**, your code may fail
* Always initialize max safely

---

# 🧠 Simple Thinking Pattern

👉 For each row:

```text
calculate avg → compare → update max
```

---

# 🎯 Key Idea

👉
**running maximum pattern**

```awk
if (value > max)
```

---



---

# 💪 Practice Check

If 2 students have same max average?

👉 What will your code do?

---

Answer that → you’ve mastered **topper/max problems** 🚀
