# 🏆 Find Topper Roll Number per School using AWK

---

## 📝 Problem Statement

Write an **AWK script (`script.awk`)** that:

* Reads student records from a CSV file

* Each line contains:

  * `School_code`
  * `Student_Roll_no`
  * `Name`
  * `Marks`

* Identifies the **topper (highest marks) for each school**

* Prints the **roll number of the topper** for each school

---

## 📥 Sample Input

```text id="tp1"
D,D33139341,Dwight,60
A,A369253,Harrison,401
D,D7779266,Hattie,491
B,B31354967,Tommie,39
D,D6371719,Brendon,296
D,D24516910,Dwight,267
D,D97274653,Hassan,464
B,B93518088,Anna,274
B,B10154966,Kent,83
B,B5350590,Kent,431
```

---

## 📤 Sample Output

```text id="tp2"
A369253
B5350590
D7779266
```

---

## ✅ Solution — script.awk

```awk id="tp3"
#!/usr/bin/gawk -f

BEGIN {
    FS = ","
}

{
    school = $1
    roll_number = $2
    marks = $4

    if (marks > max[school]) {
        max[school] = marks
        max_student[school] = roll_number
    }
}

END {
    for (s in max_student) {
        print max_student[s]
    }
}
```

---

## 🔍 Explanation

### 🔹 Field Extraction

```awk id="tp4"
school = $1
roll_number = $2
marks = $4
```

* Extracts values from each column

---

### 🔹 Track Maximum per School

```awk id="tp5"
if (marks > max[school])
```

* Stores highest marks for each school

---

### 🔹 Store Topper Roll Number

```awk id="tp6"
max_student[school] = roll_number
```

* Keeps topper’s roll number

---

### 🔹 Final Output

```awk id="tp7"
for (s in max_student)
```

* Prints topper from each school

---

## 🎯 Key Idea

👉 Use **associative arrays with school as key**

---

## ⚠️ Important Points

* One topper per school
* Output order does not matter
* Uses `max[school]` pattern

---

## 🧠 Memory Trick

👉 **school → max marks → store roll number**

---

💪 This is a **top-level exam pattern (group + max)** — master this and many problems become easy 🚀
