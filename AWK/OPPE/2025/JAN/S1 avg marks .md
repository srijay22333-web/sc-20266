# 📘 Compute Average Marks from TSV (AWK)

---

## 📝 Problem Statement

You are given a **TSV file** with columns:

```text id="avg1"
ID   Name   Mark1   Mark2   Mark3   Total
```

---

### 🎯 Your Task

* Read from **standard input**
* Extract first 5 fields
* Compute:

```text id="avg2"
average = (Mark1 + Mark2 + Mark3) / 3
```

* Print:

```text id="avg3"
ID Name average
```

👉 Output must be **space-separated**

---

## 📥 Sample Input

```text id="avg4"
1	Bob	80	75	90	245
2	Alice	88	92	85	265
3	Charlie	78	82	79	239
```

---

## 📤 Expected Output

```text id="avg5"
1 Bob 81.67
2 Alice 88.33
3 Charlie 79.33
```

---

# ✅ Clean & Final Solution

```awk id="avg6"
#!/usr/bin/gawk -f

BEGIN {
    FS = "\t"
    OFS = " "
}

{
    avg = ($3 + $4 + $5) / 3
    printf "%s %s %.2f\n", $1, $2, avg
}
```

---

# 🔍 Improvements over Your Code

---

## ✅ 1. Removed unnecessary variables

❌ Your version:

```awk id="avg7"
id = $1
name = $2
```

✔ Not needed — directly use `$1`, `$2`

---

## ✅ 2. Removed empty END block

```awk id="avg8"
END {}
```

👉 unnecessary → remove

---

## ✅ 3. Shorter & cleaner

```awk id="avg9"
avg = ($3 + $4 + $5) / 3
```

---

# 🧠 Simple Thinking

👉 Each row:

```text id="avg10"
take marks → average → print
```

---

# 🎯 Key Idea

👉
**field extraction + calculation + formatted print**

---

# ⚠️ Important Points

* `FS="\t"` → tab input
* `printf "%.2f"` → 2 decimal places
* No header → process all rows

---

# 🧠 Memory Trick

👉
**$3+$4+$5 → /3 → print**

---

# 🚀 Final Insight

This is a **basic AWK pattern**:

```text id="avg11"
read → compute → format → print
```

---

