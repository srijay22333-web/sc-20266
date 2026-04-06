## Print CSV Column Names with Index using AWK

---

# Problem Statement

You are given a CSV file with multiple columns.

Remembering column numbers manually can be difficult while writing AWK scripts.
So, you need to write a preprocessing script that:

👉 Reads the CSV file
👉 Extracts the **header row**
👉 Prints each column name along with its **column number**

---

# Input Format

* CSV data is passed through **standard input**
* First line contains **column names (header)**

---

## Sample Input

```csv
name,age,gender,city
Bob,21,M,New York
Alice,22,F,Los Angeles
```

---

## Sample Output

```text
1 name
2 age
3 gender
4 city
```

---

# Solution

```bash
#!/usr/bin/gawk -f

BEGIN {
    FS = ","
}

NR == 1 {
    for (i = 1; i <= NF; i++) {
        print i, $i
    }
}
```

---

# Explanation

### 🔹 Field Separator

```awk
FS = ","
```

👉 Splits each line into fields using comma

---

### 🔹 Process Only Header Row

```awk
NR == 1
```

👉 Executes only for the **first line** (header)

---

### 🔹 Loop Through Columns

```awk
for (i = 1; i <= NF; i++)
```

* `NF` → number of fields in current row
* Iterates through all columns

---

### 🔹 Print Column Number and Name

```awk
print i, $i
```

👉 Output format:

```text
column_number column_name
```

---

## 🧠 Key Concepts

| Feature | Meaning               |
| ------- | --------------------- |
| `FS`    | field separator       |
| `NR`    | current record number |
| `NF`    | number of fields      |
| `$i`    | ith column            |

---

## ⚡ Example Run

```bash
awk -f script.awk < file.csv
```

---

## 🧠 Key Insight

👉 This script helps you **quickly map column names to positions**, making future AWK scripts easier to write.

---

