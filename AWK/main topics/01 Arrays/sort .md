# 🧠 AWK `asort()` & `asorti()` — The Complete Exam Guide

> _"If you can't explain it simply, you don't understand it well enough."_ — Feynman

---

## 📌 Introduction — What Problem Do They Solve?

AWK arrays are **unordered**. When you store data, AWK throws it into a bag — no sequence, no order.

`asort()` and `asorti()` are your **sorting tools** to bring order to that chaos.

| Function    | What it sorts | What happens to keys |
|-------------|--------------|----------------------|
| `asort()`   | **Values**   | Keys are **destroyed** (replaced with 1, 2, 3…) |
| `asorti()`  | **Keys**     | Keys become **values** (original values lost) |

> ⚠️ **Golden Rule:** Sorting always **destroys** something. You must choose what to keep.

---

## 🎯 Simple Analogy — Explain Like I'm 10

### Imagine a Classroom Register

```
Roll No → Student Name
A       → Zara
C       → Amit  
B       → Priya
```

**`asort()` = Sort by Student Names (Values)**

- You tear out the names, sort them alphabetically: Amit, Priya, Zara
- But now the roll numbers are gone! You relabel them 1, 2, 3
- ❌ Roll numbers lost

**`asorti()` = Sort by Roll Numbers (Keys)**

- You sort the roll numbers: A, B, C
- These sorted roll numbers are now stored as values in a new array
- ✅ You can still look up the original array using these sorted keys

> 🧠 **Think of it this way:**
> - `asort` = "I only care about the **answers** (values), forget the labels"
> - `asorti` = "I care about the **labels** (keys), give me them in order"

---

## 📗 Example 1 — Basic Array

### Starting Array

```
arr["A"] = 50
arr["B"] = 80
arr["C"] = 30
```

| Key | Value |
|-----|-------|
| A   | 50    |
| B   | 80    |
| C   | 30    |

---

### Using `asort(arr, sorted)`

**What happens:** Values are sorted → Keys become 1, 2, 3

```awk
BEGIN {
    arr["A"] = 50
    arr["B"] = 80
    arr["C"] = 30

    n = asort(arr, sorted)

    for (i = 1; i <= n; i++)
        print i, sorted[i]
}
```

**Output:**
```
1 30
2 50
3 80
```

#### Before vs After — `asort()`

| **BEFORE (arr)**  |       | **AFTER (sorted)** |       |
|-------------------|-------|--------------------|-------|
| Key               | Value | Key                | Value |
| A                 | 50    | 1                  | 30    |
| B                 | 80    | 2                  | 50    |
| C                 | 30    | 3                  | 80    |

> ❌ **Keys A, B, C are GONE.** You cannot trace which value belonged to which key.

---

### Using `asorti(arr, sorted)`

**What happens:** Keys are sorted → Sorted keys stored as values in `sorted`

```awk
BEGIN {
    arr["A"] = 50
    arr["B"] = 80
    arr["C"] = 30

    n = asorti(arr, sorted)

    for (i = 1; i <= n; i++)
        print i, sorted[i], "→", arr[sorted[i]]
}
```

**Output:**
```
1 A → 50
2 B → 80
3 C → 30
```

#### Before vs After — `asorti()`

| **BEFORE (arr)**  |       | **AFTER (sorted)**  |       | **Lookup arr[sorted[i]]** |
|-------------------|-------|---------------------|-------|---------------------------|
| Key               | Value | Key                 | Value | Original Value            |
| A                 | 50    | 1                   | A     | 50                        |
| B                 | 80    | 2                   | B     | 80                        |
| C                 | 30    | 3                   | C     | 30                        |

> ✅ **Original array `arr` is UNTOUCHED.** You can still access `arr["A"]` = 50.

---

## 📗 Example 2 — Descending Sort

AWK (gawk) supports a **third argument** to control sort order.

### Common Sort Orders

| Sort Specifier       | Meaning                        |
|----------------------|--------------------------------|
| `@val_num_asc`       | Values ↑ numerically (default) |
| `@val_num_desc`      | Values ↓ numerically           |
| `@val_str_asc`       | Values ↑ as strings            |
| `@val_str_desc`      | Values ↓ as strings            |
| `@ind_num_asc`       | Keys ↑ numerically             |
| `@ind_num_desc`      | Keys ↓ numerically             |
| `@ind_str_asc`       | Keys ↑ as strings              |
| `@ind_str_desc`      | Keys ↓ as strings              |

> 💡 `val` = value, `ind` = index (key), `num` = numeric, `str` = string

---

### Descending by Values — `@val_num_desc`

```awk
BEGIN {
    arr["A"] = 50
    arr["B"] = 80
    arr["C"] = 30

    n = asort(arr, sorted, "@val_num_desc")

    for (i = 1; i <= n; i++)
        print i, sorted[i]
}
```

**Output:**
```
1 80
2 50
3 30
```

| Key | Value |
|-----|-------|
| 1   | 80    |
| 2   | 50    |
| 3   | 30    |

---

### Descending by Keys — `@ind_str_desc`

```awk
BEGIN {
    arr["A"] = 50
    arr["B"] = 80
    arr["C"] = 30

    n = asorti(arr, sorted, "@ind_str_desc")

    for (i = 1; i <= n; i++)
        print i, sorted[i], "→", arr[sorted[i]]
}
```

**Output:**
```
1 C → 30
2 B → 80
3 A → 50
```

---

## 📗 Example 3 — Real Exam Use Case: Student Marks

### Problem Statement

> Given student data, print names sorted by roll number.

**Input File (`students.txt`):**
```
103 Zara 85
101 Amit 92
102 Priya 78
```

---

### ❌ WRONG Approach — Using `asort()`

```awk
{
    marks[$1] = $2 " " $3
}
END {
    n = asort(marks, sorted)
    for (i = 1; i <= n; i++)
        print sorted[i]
}
```

**Output:**
```
Amit 92
Priya 78
Zara 85
```

> ❌ **Problem:** Roll numbers are LOST. You cannot print which roll number belongs to whom.

---

### ✅ CORRECT Approach — Using `asorti()`

```awk
{
    marks[$1] = $2 " " $3
}
END {
    n = asorti(marks, sorted)
    for (i = 1; i <= n; i++)
        print sorted[i], marks[sorted[i]]
}
```

**Output:**
```
101 Amit 92
102 Priya 78
103 Zara 85
```

> ✅ **Roll numbers preserved and sorted!** This is why `asorti()` wins in exams.

---

### Bonus: Sort by Marks (Descending) — Top Scorer First

```awk
{
    marks[$2] = $3    # name → marks
    roll[$2]  = $1    # name → roll
}
END {
    n = asort(marks, sorted, "@val_num_desc")
    # But wait — we lost the names!
    # Better approach below:
}
```

**Better Pattern — Keep Everything:**

```awk
{
    data[$3 " " $1] = $2   # "marks rollno" → name
}
END {
    n = asorti(data, sorted, "@ind_num_desc")
    for (i = 1; i <= n; i++) {
        split(sorted[i], parts, " ")
        print parts[2], data[sorted[i]], parts[1]
    }
}
```

---

## 🔑 Key Differences — Side by Side

| Feature                  | `asort()`                          | `asorti()`                          |
|--------------------------|------------------------------------|--------------------------------------|
| **Sorts**                | Values                             | Keys (indices)                       |
| **Result contains**      | Sorted values                      | Sorted keys (as values)              |
| **Original keys**        | ❌ DESTROYED                       | ✅ Available via lookup              |
| **Original values**      | ✅ Preserved (but re-keyed 1,2,3)  | ✅ Accessible via `arr[sorted[i]]`   |
| **New keys in result**   | 1, 2, 3, …                        | 1, 2, 3, …                          |
| **Can access original?** | ❌ No way to trace back            | ✅ `arr[sorted[i]]` gives value      |
| **Exam usage**           | ~10% of cases                      | ~90% of cases                        |
| **When to use**          | "Just give me sorted values"       | "Sort keys, keep the mapping alive"  |

---

## 🧭 When to Use Which — Exam Decision Tree

```
Do you need to keep the key-value relationship?
│
├── YES → use asorti()
│         Pattern: asorti(arr, sorted)
│         Access:  arr[sorted[i]]
│
└── NO  → use asort()
          Pattern: asort(arr, sorted)
          Access:  sorted[i]
```

### Quick Rules

| Exam Question Says…                         | Use           |
|----------------------------------------------|--------------|
| "Sort by roll number and print marks"        | `asorti()`   |
| "Sort students by name"                      | `asorti()`   |
| "Print marks in ascending order"             | `asort()`    |
| "Sort and print with original labels"        | `asorti()`   |
| "Find top 3 values"                          | `asort()`    |
| "Sort by key and access values"              | `asorti()`   |

> 📝 **Exam Shortcut:** If the question asks you to print **both key and value** after sorting → **always `asorti()`**.

---

## ⚠️ Common Mistakes

### Mistake 1: Using `asort()` when you need keys

```awk
# ❌ WRONG — keys are lost after asort()
n = asort(arr, sorted)
for (i = 1; i <= n; i++)
    print sorted[i], arr[sorted[i]]   # arr[sorted[i]] won't work!
```

> `sorted[i]` is a **value** (e.g., 50), not a key. `arr[50]` doesn't exist!

### Mistake 2: Forgetting the second argument

```awk
# ⚠️ DANGER — this OVERWRITES the original array!
asort(arr)

# Now arr's keys are 1, 2, 3 and original keys are gone forever
```

> ✅ **Always use two arguments:** `asort(arr, sorted)` — keeps `arr` safe.

### Mistake 3: Confusing sorted output keys

```awk
# After asorti(arr, sorted):
# sorted[1] = "A"    ← this is a KEY from arr, stored as VALUE
# sorted[2] = "B"
# sorted[3] = "C"

# To get the original value:
arr[sorted[1]]        # ✅ = 50
sorted[1]             # ❌ = "A" (this is a key, not a value)
```

### Mistake 4: Numeric vs String sorting

```awk
# Keys: "1", "2", "10", "20"

asorti(arr, sorted)
# String sort: 1, 10, 2, 20  ← WRONG for numbers!

asorti(arr, sorted, "@ind_num_asc")
# Numeric sort: 1, 2, 10, 20  ← CORRECT
```

> ⚠️ **AWK treats keys as strings by default.** Use `@ind_num_asc` for numeric keys.

---

## 🧩 Complete Code Patterns — Copy-Paste Ready

### Pattern 1: Sort values (ascending)
```awk
n = asort(arr, sorted)
for (i = 1; i <= n; i++) print sorted[i]
```

### Pattern 2: Sort values (descending)
```awk
n = asort(arr, sorted, "@val_num_desc")
for (i = 1; i <= n; i++) print sorted[i]
```

### Pattern 3: Sort keys, access values
```awk
n = asorti(arr, sorted)
for (i = 1; i <= n; i++) print sorted[i], arr[sorted[i]]
```

### Pattern 4: Sort keys descending, access values
```awk
n = asorti(arr, sorted, "@ind_str_desc")
for (i = 1; i <= n; i++) print sorted[i], arr[sorted[i]]
```

### Pattern 5: Sort numeric keys properly
```awk
n = asorti(arr, sorted, "@ind_num_asc")
for (i = 1; i <= n; i++) print sorted[i], arr[sorted[i]]
```

---

## 📝 Practice Questions

### Q1: What is the output?

```awk
BEGIN {
    a["X"] = 100
    a["Y"] = 50
    a["Z"] = 75
    n = asort(a, b)
    for (i = 1; i <= n; i++) print i, b[i]
}
```

<details>
<summary>✅ Answer</summary>

```
1 50
2 75
3 100
```
Values sorted ascending. Keys X, Y, Z are lost.
</details>

---

### Q2: What is the output?

```awk
BEGIN {
    a["banana"] = 3
    a["apple"]  = 5
    a["cherry"] = 1
    n = asorti(a, b)
    for (i = 1; i <= n; i++) print b[i], a[b[i]]
}
```

<details>
<summary>✅ Answer</summary>

```
apple 5
banana 3
cherry 1
```
Keys sorted alphabetically. Values accessed via `a[b[i]]`.
</details>

---

### Q3: Fix this buggy code

> Goal: Print roll numbers and names sorted by roll number.

```awk
{
    student[$1] = $2
}
END {
    n = asort(student, sorted)
    for (i = 1; i <= n; i++)
        print sorted[i], student[sorted[i]]
}
```

<details>
<summary>✅ Answer</summary>

**Bug:** `asort()` sorts values (names), not keys (roll numbers). After sorting, `student[sorted[i]]` won't work because `sorted[i]` contains a name, not a roll number.

**Fix:** Replace `asort` with `asorti`:

```awk
{
    student[$1] = $2
}
END {
    n = asorti(student, sorted)
    for (i = 1; i <= n; i++)
        print sorted[i], student[sorted[i]]
}
```
</details>

---

## 🧠 Memory Trick — One Line to Remember

```
asort  = sort VALUES    → keys DIE     → "a" for "all values, keys abandoned"
asorti = sort INDICES   → keys LIVE    → "i" for "indices in order"
```

> 🎯 **The "i" in `asorti` = "indices" = KEYS. That's all you need to remember.**

---

## ⚡ Exam Shortcut Summary

| # | Rule |
|---|------|
| 1 | `asort()` → values sorted, keys become 1,2,3 → **original keys LOST** |
| 2 | `asorti()` → keys sorted and stored as values → **original data SAFE** |
| 3 | Always use **2 arguments**: `asort(arr, sorted)` — never destroy original |
| 4 | To access original values after `asorti()`: use `arr[sorted[i]]` |
| 5 | For numeric keys, add `"@ind_num_asc"` as 3rd argument |
| 6 | 90% of exam questions need `asorti()` — when in doubt, use `asorti()` |
| 7 | If question says "print key and value sorted" → **always `asorti()`** |

---

> 💡 **Final Tip:** In exams, write `asorti()` unless the question **explicitly** says "sort only the values and don't care about keys." That one rule will save you in 90% of questions.

---

*Made with ❤️ for exam success — using the Feynman technique.*
