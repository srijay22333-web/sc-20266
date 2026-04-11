# 📄 Find File with Maximum Lines using AWK (Using Given Code)

---

## 📝 Problem Statement

Write an **AWK script** that:

* Reads multiple files
* Counts the number of lines in each file
* Prints:

  * The **file with the maximum number of lines**
* If multiple files have the same maximum:

  * Print the **first one encountered**

---

## 📥 Sample Input

```text
file1.txt
1
2
3
4
5

file2.txt
1
2
3

file3.txt
1
```

---

## 📤 Sample Output

```text
file1.txt
```

---

## 💻 Solution (big.awk)

```awk id="g1v3n"
FNR == 1 {
    file_line_count[FILENAME] = 0
    order[++idx] = FILENAME
}

{
    file_line_count[FILENAME]++
}

END {
    max = 0
    for (i = 1; i <= idx; i++) {
        f = order[i]
        if (file_line_count[f] > max) {
            max = file_line_count[f]
            max_file = f
        }
    }
    print max_file
}
```

---

## 🔍 Explanation

### 🔹 Problem in Given Code

```awk id="prblm"
for (f in file_line_count)
```

* AWK **does NOT guarantee order** of array traversal
* So "first file" condition may break

---

### 🔹 Fix: Maintain Order

```awk id="ordr"
order[++idx] = FILENAME
```

* Stores files in the order they appear

---

### 🔹 Iterate in Order

```awk id="itr"
for (i = 1; i <= idx; i++)
```

* Ensures files are checked in correct sequence

---

### 🔹 Keep First Maximum

```awk id="mx"
if (file_line_count[f] > max)
```

* Uses `>` (not `>=`) to preserve first occurrence

---

## 🎯 Key Idea

👉 Arrays are unordered → Track order separately

---

## ⚠️ Important Points

* Never rely on `for (f in array)` for ordered results
* Use an auxiliary array (`order[]`) to preserve sequence
* Keep comparison strictly `>` for tie handling

---

## 🧠 Memory Trick

👉 **Store order → Loop in order → Pick max once**
