# 📘 Weekly & Daily Total Production using AWK

---

## 📝 Problem Statement

You are given a CSV file where:

* Each row represents a **week**
* Each column represents production for a **day (Monday → Sunday)**
* There are always **7 columns**

---

### 🎯 Your Task

1. For each row:

   * Compute the **total production of that week**
   * Append it as a new column

2. For all rows:

   * Compute the **total production for each day**
   * Print it as the **last row**

---

## 📥 Sample Input

```text id="tp1"
100,150,200,250,300,350,400
110,160,210,260,310,360,410
120,170,220,270,320,370,420
130,180,230,280,330,380,430
```

---

## 📤 Sample Output

```text id="tp2"
100,150,200,250,300,350,400,1750
110,160,210,260,310,360,410,1820
120,170,220,270,320,370,420,1890
130,180,230,280,330,380,430,1960
460,660,860,1060,1260,1460,1660
```

---

## 💻 Solution (script.awk)

```awk id="tp3"
#!/usr/bin/gawk -f

BEGIN {
    FS = ","
    OFS = ","
}

{
    summ = 0

    for (i = 1; i <= NF; i++) {
        summ += $i          # row total (week)
        col[i] += $i        # column total (day)
    }

    print $0, summ          # print row + total
}

END {
    for (i = 1; i <= 7; i++) {
        printf "%s", col[i]

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

```awk id="tp4"
FS = ","
OFS = ","
```

👉 Input and output are CSV format

---

### 🔹 Loop Through Columns

```awk id="tp5"
for (i = 1; i <= NF; i++)
```

👉 Iterates through all 7 days

---

### 🔹 Row Total

```awk id="tp6"
summ += $i
```

👉 Calculates total production for a week

---

### 🔹 Column Total

```awk id="tp7"
col[i] += $i
```

👉 Stores total production for each day

---

### 🔹 Print Row Output

```awk id="tp8"
print $0, summ
```

👉 Appends weekly total

---

### 🔹 Final Output (END Block)

```awk id="tp9"
col[i]
```

👉 Prints total production for each day

---

## 🎯 Key Idea

👉 Use loop + array:

* `summ` → row total
* `col[i]` → column total

---

## ⚠️ Important Points

* Use `END` block for final output
* `printf` used to control commas
* Works only for 7 columns

---

## 🧠 Memory Trick

👉

* Row → summ
* Column → col[i]

---

## 🚀 Final Insight

This problem teaches:

* Associative arrays
* Row + column aggregation
* END block usage

---
