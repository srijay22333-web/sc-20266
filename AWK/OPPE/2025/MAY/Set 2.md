# 📘 Weekly & Daily Average Production using AWK

---

## 📝 Problem Statement

You are given a CSV file where:

* Each row represents a **week**
* Each column represents production for a **day (Monday → Sunday)**
* There are always **7 columns**

---

### 🎯 Your Task

1. For each row:

   * Compute the **average production of that week**
   * Append it as a new column

2. For all rows:

   * Compute the **average production for each day**
   * Print it as the **last row**

---

## 📥 Sample Input

```text id="ap1"
100,150,200,255,300,350,400
110,160,210,260,310,360,410
120,170,220,270,320,370,420
130,180,230,280,330,380,430
```

---

## 📤 Sample Output

```text id="ap2"
100,150,200,255,300,350,400,250.714
110,160,210,260,310,360,410,260
120,170,220,270,320,370,420,270
130,180,230,280,330,380,430,280
115,165,215,266.25,315,365,415
```

---

## 💻 Solution (script.awk)

```awk id="ap3"
#!/usr/bin/gawk -f

BEGIN {
    FS = ","
    OFS = ","
}

{
    summ = 0

    for (i = 1; i <= NF; i++) {
        summ += $i          # row total
        col[i] += $i        # column total
    }

    print $0, summ / NF     # row average

    count++                 # number of rows
}

END {
    for (i = 1; i <= 7; i++) {
        printf "%s", col[i] / count

        if (i < 7) {
            printf ","
        }
    }

    printf "\n"
}
```

---

## 🔍 Explanation

---

### 🔹 Field Separator

```awk id="ap4"
FS = ","
OFS = ","
```

👉 CSV input and output

---

### 🔹 Loop Through Columns

```awk id="ap5"
for (i = 1; i <= NF; i++)
```

👉 Iterates all 7 days

---

### 🔹 Row Sum

```awk id="ap6"
summ += $i
```

👉 Total production of the week

---

### 🔹 Column Sum

```awk id="ap7"
col[i] += $i
```

👉 Total production per day

---

### 🔹 Row Average

```awk id="ap8"
summ / NF
```

👉 Weekly average

---

### 🔹 Row Count

```awk id="ap9"
count++
```

👉 Number of weeks

---

### 🔹 Column Average (END Block)

```awk id="ap10"
col[i] / count
```

👉 Daily average

---

## 🎯 Key Idea

👉 Use same logic as total:

* Just divide at the right place

---

## ⚠️ Important Points

* `NF` → number of columns (7)
* `count` → number of rows
* Use `END` for final result

---

## 🧠 Memory Trick

👉

* Row → summ / NF
* Column → col[i] / count

---

## 🚀 Final Insight

This problem teaches:

* Reuse logic (sum → average)
* Aggregation + division
* Clean AWK structure

---
