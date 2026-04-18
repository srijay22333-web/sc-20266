# 📘 Total Sales Per Person (Sorted)

---

## 📝 Problem Statement

Compute total sales per person from a file and sort the results.

---

## 🎯 Task

* Calculate total sales per person
* Sort results (based on sales values)

---

## 📥 Sample Input

```text id="1q3l7u"
Alice 200
Bob 150
Alice 300
Charlie 400
Bob 100
```

---

## 📤 Sample Output

```text id="5zq0h2"
Bob 250
Charlie 400
Alice 500
```

---

## ⚠️ Important Note

Your current script uses:

```awk id="h4g6ks"
"@val_num_asc"
```

👉 This sorts in **ascending order**, which matches the sample output.
(If descending was required, it should be `@val_num_desc`)

---

## ✅ Solution — script.awk

```awk id="9g9zv2"
BEGIN {
    FS = " "
}

{
    sales[$1] += $2
}

END {
    n = asorti(sales, sorted, "@val_num_asc")

    for (i = 1; i <= n; i++) {
        name = sorted[i]
        print name, sales[name]
    }
}
```

---

## ▶️ Run

```bash id="y5vh6n"
awk -f script.awk sales.txt
```

---

## 🔍 Explanation

* `FS = " "` → split line into fields
* `sales[$1] += $2` → accumulate total sales per person
* `asorti(... "@val_num_asc")` → sort by sales (ascending)
* `sorted[i]` → holds names in sorted order
* loop → prints results

---

## 🧠 Visual Understanding

### ▶ Accumulation Phase

```text id="k9v4h0"
Alice   → 200 → 500
Bob     → 150 → 250
Charlie → 400
```

---

### ▶ Sorting (Ascending)

```text id="d2r8sm"
Bob     → 250
Charlie → 400
Alice   → 500
```

---

## 🔥 Key Concept

```text id="p7z1gm"
asorti(array, result, "@val_num_asc") → sort by values
```

---

## ✅ Alternative (using sort command)

```awk id="x0m8qs"
BEGIN { FS = " " }
{
    sales[$1] += $2
}
END {
    for (name in sales) {
        print name, sales[name]
    }
}
```

### ▶️ Run

```bash id="k7d9wx"
awk -f script.awk sales.txt | sort -k2 -n
```
