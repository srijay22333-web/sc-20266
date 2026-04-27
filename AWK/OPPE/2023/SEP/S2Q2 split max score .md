# 📘 Highest Score per Student (AWK)

---

## 📝 Problem Statement

You are given a CSV file where each row contains:

```csv
timestamp,roll_number,evaluation_details
```

---

### 📌 Field Meaning

* `timestamp` → submission time
* `roll_number` → student ID
* `evaluation_details` → format `marks/total`

---

## 🎯 Your Task

For each student:

* Convert score to percentage:

```text
score = (marks / total) * 100
```

* Consider **only highest score** among multiple submissions
* Print:

```csv
roll_number,score
```

* Output must be **sorted by roll_number**

---

## 📥 Sample Input

```text id="sc1"
2023-09-01 09:00:00,1921821,11/20
2023-09-01 09:01:37,1921822,15/20
2023-09-01 09:02:00,1921821,15/20
2023-09-01 09:03:00,1921822,20/20
2023-09-01 09:04:00,1921823,20/25
```

---

## 📤 Sample Output

```text id="sc2"
1921821,75
1921822,100
1921823,80
```

---

# 💻 Solution (script.awk)

```awk
#!/usr/bin/gawk -f

BEGIN {
    FS = ","
    OFS = ","
}

{
    split($3, arr, "/")                 # split marks/total
    score = (arr[1] / arr[2]) * 100     # compute percentage

    if (score > max[$2])                # keep highest score
        max[$2] = score
}

END {
    # sort by roll number
    n = asorti(max, sorted)

    for (i = 1; i <= n; i++) {
        roll = sorted[i]
        print roll, max[roll]
    }
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 Extract marks

```awk
split($3, arr, "/")
```

👉 `11/20 → arr[1]=11, arr[2]=20`

---

## 🔹 Compute score

```awk
score = (arr[1] / arr[2]) * 100
```

---

## 🔹 Track highest

```awk
max[$2]
```

👉 `$2` = roll number

---

## 🔹 Sort output

```awk
asorti(max, sorted)
```

👉 sorts roll numbers

---

## 🔹 Print

```awk
print roll, max[roll]
```

---

# 🎯 Key Idea

👉
**roll → max score mapping**

---

# ⚠️ Important Points

* Use `split()` for fraction
* Use associative array `max[]`
* Use `asorti()` for sorting

---

# 🧠 Memory Trick

👉
**split → compute → max → sort → print**

---

# 🚀 Final Insight

This is a **real OPPE pattern**:

```text
group → compute → keep best → sort → output
```

---


If you answer correctly → you’ve mastered **max-per-key problems** 🚀
