# 📘 Sum + Maximum Course per Student (AWK)

---

## 📝 Problem Statement

You are given input:

```text
Name Course Marks
```

---

### 🎯 Your Task

* For each `(name, course)`:

  * Sum all marks
* For each `name`:

  * Find course with **maximum total marks**
* Print:

```text
Name BestCourse
```

---

## 📥 Sample Input

```text id="cmb1"
Alice math 10
Alice math 20
Alice science 15
Bob math 30
Bob science 5
Bob science 25
```

---

## 📤 Expected Output

```text id="cmb2"
Alice math
Bob science
```

---

# 💻 Corrected Solution (Your Code — FIXED ONLY)

```awk
#!/usr/bin/gawk -f

BEGIN {
	FS = " "
}

{
    name = $1
    course = $2
    marks = $3 + 0

    group[name, course] += marks

    # ✅ FIX: use > (max), not <
    if (!(name in max) || group[name, course] > max[name]) {
        max[name] = group[name, course]
        best[name] = course
    }
}

END {
    for (k in best)
        print k, best[k]
}
```

---

# 🔍 What Was Wrong?

---

## ❌ Your condition

```awk
group[name, course] < max[name]
```

👉 This finds **minimum**, not maximum

---

## ✅ Correct condition

```awk
group[name, course] > max[name]
```

👉 Finds **maximum total marks**

---

# 🧠 Step-by-Step Visualization

---

## 🔹 Build totals

| Name  | Course  | Total |
| ----- | ------- | ----- |
| Alice | math    | 30    |
| Alice | science | 15    |
| Bob   | math    | 30    |
| Bob   | science | 30    |

---

## 🔹 Pick max per name

* Alice → **math (30)**
* Bob → **science (30)** *(last wins in tie)*

---

# 🎯 Key Idea

👉
**(name, course) → sum → compare → store best**

---

# ⚠️ Important Points

* `group[name, course]` → 2D key
* `max[name]` → best score per person
* `best[name]` → best course

---

# 🧠 Memory Trick

👉
**sum first → then max**
