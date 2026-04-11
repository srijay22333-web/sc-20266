# 📊 Calculate Subject-wise Class Average using AWK

---

## 📝 Problem Statement

Write an **AWK script** that:

* Reads data from `marks.csv`
* Calculates **class average** for:

  * Mathematics
  * Physics
  * Chemistry
* Prints:

  * Each subject average on a **new line**
  * In the order: **Maths, Physics, Chemistry**

---

## 📥 Sample Input (`marks.csv`)

```text
Name,Maths,Physics,Chemistry
Jude,100,95,85
Gayathri,85,99,92
Priya,95,92,100
Hari,97,89,99
Ruban,87,98,95
Madhu,85,100,96
```

---

## 📤 Sample Output

```text
91.5
95.5
94.5
```

---

## 💻 Solution (file.awk)

```awk id="avgfix"
BEGIN {
    FS = ","
}

NR > 1 {
    math_sum += $2
    physics_sum += $3
    chemistry_sum += $4
    count++
}

END {
    print math_sum / count
    print physics_sum / count
    print chemistry_sum / count
}
```

---

## 🔍 Explanation

### 🔹 Set Field Separator

```awk
FS = ","
```

* CSV file → values separated by commas

---

### 🔹 Skip Header Row

```awk
NR > 1
```

* Ignores first line (column names)

---

### 🔹 Add Marks

```awk
math_sum += $2
physics_sum += $3
chemistry_sum += $4
```

* Adds marks for each subject

---

### 🔹 Count Students

```awk
count++
```

* Tracks number of records

---

### 🔹 Calculate Average

```awk
print math_sum / count
```

* Done only once in `END` block

---

## 🎯 Key Idea

👉 **Sum → Count → Divide at the end**

---

## ⚠️ Important Points

* Do NOT calculate average inside loop
* Avoid using `NR-1` repeatedly
* Keep logic simple and clean

---

## 🧠 Memory Trick

👉 **Add everything → divide once at end**
