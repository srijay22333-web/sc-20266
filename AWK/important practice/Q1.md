# 📘 Count Passed Students per Course

---

# Problem Statement

You are given a CSV file containing student records with the following fields:

```text
roll_no,course_code,course_name,term_code,grade
```

Your task is to:

* Count the number of students who **passed** in each course
* A student is considered **passed** if grade is NOT:

  * `WQ`, `U`, `F`, `I_OP`
* Print the result in the format:

```text
Course Code,Passed Students
```

---

# Sample Input

```text id="pc1"
roll_no,course_code,course_name,term_code,grade
27f7034476,CS1002,CS1002 - Embedded C Programming,F3-2026,WQ
27f1877386,CS1002,CS1002 - Embedded C Programming,F2-2025,U
27f6600140,CS2001,CS2001 - Python Programming,F2-2025,A
29f7986047,CS2001,CS2001 - Python Programming,F2-2026,WQ
26f6674179,CS1001,CS1001 - C Programming,F3-2025,E
27f3111167,CS2001,CS2001 - Python Programming,F3-2025,U
26f1556539,CS1102,CS1102 - Linux Shell,F1-2026,F
28f2194770,CS1001,CS1001 - C Programming,F2-2026,E
27f1680285,CS1002,CS1002 - Embedded C Programming,F3-2025,C
29f7322429,CS1002,CS1002 - Embedded C Programming,F1-2025,WQ
28f9472772,CS1002,CS1002 - Embedded C Programming,F2-2026,B
27f2756423,CS2001,CS2001 - Python Programming,F1-2026,I_OP
26f7527707,CS1002,CS1002 - Embedded C Programming,F2-2025,E
```

---

# Sample Output

```text id="pc2"
Course Code,Passed Students
CS1002,3
CS2001,1
CS1001,2
CS1102,1
```

---

# 🔹 Solution 1 — Without Order Preservation

```bash id="pc3"
awk '
BEGIN {
    FS = ","
}

NR == 1 { next }

{
    course = $2
    grade = $5

    if (grade != "WQ" && grade != "U" && grade != "F" && grade != "I_OP") {
        count[course]++
    }
}

END {
    print "Course Code,Passed Students"

    for (c in count) {
        print c "," (count[c] + 0)
    }
}
'
```

---

# Explanation

* `FS=","` → splits CSV fields
* `NR==1 {next}` → skips header
* `count[course]++` → counts passed students

---

### ⚠️ Limitation

```awk id="pc4"
for (c in count)
```

👉 Output order is **random**
👉 Does NOT match input order

---

# 🔹 Solution 2 — Preserve Input Order (Recommended)

```bash id="pc5"
awk '
BEGIN {
    FS = ","
}

NR == 1 { next }

{
    course = $2
    grade = $5

    if (!(course in seen)) {
        seen[course] = 1
        order[++n] = course
    }

    if (grade != "WQ" && grade != "U" && grade != "F" && grade != "I_OP") {
        count[course]++
    }
}

END {
    print "Course Code,Passed Students"

    for (i = 1; i <= n; i++) {
        c = order[i]
        print c "," (count[c] + 0)
    }
}
'
```

---

# Explanation

### 🔹 `seen[]`

* Tracks if course is already encountered
* Prevents duplicates

---

### 🔹 `order[]`

* Stores course order as they appear

```awk id="pc6"
order[++n] = course
```

---

### 🔹 Final Loop

```awk id="pc7"
for (i = 1; i <= n; i++)
```

👉 Ensures output is printed in **input order**

---

# ⚡ Key Concept

👉 AWK arrays are **unordered**

👉 To preserve order:

```awk id="pc8"
seen[] + order[]
```

---
