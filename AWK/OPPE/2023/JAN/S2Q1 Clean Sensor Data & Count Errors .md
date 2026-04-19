# 📘 Clean Sensor Data & Count Errors

---

## 📝 Problem Statement

A CSV file contains time-series sensor data:

```text id="sensor_cols"
time, temperature, pressure, humidity
```

Some faulty readings are marked as `"Err"`.

👉 Remove rows containing `"Err"` and print the **number of deleted rows** at the end.

---

## 🎯 Task

* Read CSV data
* Skip rows containing `"Err"`
* Print only valid rows
* Count and print number of removed rows

---

## 📥 Sample Input

```text id="sensor_in"
time,temperature,pressure,humidity
2012-10-13 12.00,304.5,29.71,80
2012-10-14 12.00,303.2,29.68,78
2012-10-15 12.00,Err,29.68,80
2012-10-16 12.00,300.9,29.71,Err
```

---

## 📤 Sample Output

```text id="sensor_out"
time,temperature,pressure,humidity
2012-10-13 12.00,304.5,29.71,80
2012-10-14 12.00,303.2,29.68,78
2
```

---

## ⚠️ Important Note

* `/Err/` → matches any row containing `"Err"`
* Header row is preserved (since it doesn’t contain `"Err"`)
* Counter tracks number of invalid rows

---

## ✅ Solution — script.awk

```awk id="awk_sensor_clean"
BEGIN {
    FS = ","
}

/!Err/ {
    print
}

/Err/ {
    ++count
}

END {
    if (count) {
        print count
    } else {
        print 0
    }
}
```

---

## ▶️ Run

```bash id="run_sensor"
awk -f script.awk input.csv
```

---

## 🔍 Explanation

* `FS=","` → sets CSV format
* `/!Err/` → prints rows without errors
* `/Err/` → counts faulty rows
* `++count` → increments deleted row counter
* `END` → prints total removed rows

---

## 🧠 Visual Understanding

### ▶ Filtering Rows

```text id="sensor_filter"
Valid   → printed ✅
Err row → removed ❌
```

---

### ▶ Counting Errors

```text id="sensor_count"
Row 3 → Err → +1
Row 4 → Err → +1
Total → 2
```

---

## 🔥 Key Concept

```text id="sensor_key"
/pattern/ → AWK pattern matching for filtering rows
```

---

## ✅ Alternative (Single Rule)

```awk id="sensor_alt"
BEGIN { FS="," }

{
    if ($0 ~ /Err/) {
        count++
    } else {
        print
    }
}

END {
    print count+0
}
```
