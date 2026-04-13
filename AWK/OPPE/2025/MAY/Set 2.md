# 📘 Weekly & Daily Average Production (Manual Column Method)

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

   * Compute the **average production per day**
   * Print it as the **last row**

---

## 📥 Sample Input

```text id="ps1"
100,150,200,255,300,350,400
110,160,210,260,310,360,410
120,170,220,270,320,370,420
130,180,230,280,330,380,430
```

---

## 📤 Sample Output

```text id="ps2"
100,150,200,255,300,350,400,250.714
110,160,210,260,310,360,410,260
120,170,220,270,320,370,420,270
130,180,230,280,330,380,430,280
115,165,215,266.25,315,365,415
```

---

## 💻 Solution (script.awk)

```awk id="ps3"
#!/usr/bin/gawk -f

BEGIN {
    FS=","; OFS=","
}

{
    # calculate row sum
    sum = $1+$2+$3+$4+$5+$6+$7

    # count rows
    row_count++

    # print row average
    print $1,$2,$3,$4,$5,$6,$7, sum/NF

    # accumulate column sums
    col1 += $1
    col2 += $2
    col3 += $3
    col4 += $4
    col5 += $5
    col6 += $6
    col7 += $7
}

END {
    print col1/row_count, col2/row_count, col3/row_count,
          col4/row_count, col5/row_count, col6/row_count, col7/row_count
}
```

---

## 🔍 Explanation

---

### 🔹 Field Separator

```awk id="ps4"
FS=","; OFS=","
```

👉 Input and output are CSV format

---

### 🔹 Row Sum

```awk id="ps5"
sum = $1+$2+$3+$4+$5+$6+$7
```

👉 Adds production of all 7 days

---

### 🔹 Row Count

```awk id="ps6"
row_count++
```

👉 Counts total weeks

---

### 🔹 Row Average

```awk id="ps7"
sum/NF
```

👉 `NF = 7` → calculates weekly average

---

### 🔹 Column Sums

```awk id="ps8"
col1 += $1
...
col7 += $7
```

👉 Stores total production per day

---

### 🔹 Column Average (END Block)

```awk id="ps9"
col1/row_count
```

👉 Computes average for each day

---

## 🎯 Key Idea

👉 Perform **two types of aggregation**:

* Row-wise → weekly average
* Column-wise → daily average

---

## ⚠️ Important Points

* Hardcoded columns (`$1` to `$7`)
* Works only when columns = 7
* Must divide by `NF` and `row_count`

---

## 🧠 Memory Trick

👉

* Row → sum → divide by NF
* Column → sum → divide by rows

---

## 🚀 Final Insight

This approach is:

* ✔ Simple to understand
* ❌ Not flexible (fixed columns)

