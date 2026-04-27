# 📘 Probability Check (not vs naught) — AWK

---

## 📝 Problem Statement

You are given a dataset where:

* Each line contains either:

  * `1` → event happened
  * `0` → event did not happen

---

### 🎯 Your Task

* Compute probability:

```text
P(X) = number of 1s / total observations
```

* Print:

```text
not      → if P(X) > 0.5  
naught   → otherwise
```

---

## 📥 Sample Input

```text
1
1
1
1
1
1
1
0
0
0
```

---

## 📤 Sample Output

```text
not
```

---

# 💻 Solution (script.awk)

```awk
#!/usr/bin/gawk -f

{
    if ($1 == 1)
        ones++
}

END {
    prob = ones / NR

    if (prob > 0.5)
        print "not"
    else
        print "naught"
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 Count occurrences of 1

```awk
ones++
```

👉 Count how many times event happened

---

## 🔹 Total trials

```awk
NR
```

👉 Total number of lines

---

## 🔹 Compute probability

```awk
prob = ones / NR
```

---

## 🔹 Decision

```awk
if (prob > 0.5)
```

👉 More probable than not

---

# ❌ Issues in Your Code

---

### ❗ 1. Unnecessary zero counting

```awk
total_zero+=1
```

👉 Not needed at all

---

### ❗ 2. Wrong logic

```awk
else if (total_zero_trails > 0)
```

👉 This is always true (there are zeros) → wrong condition

---

### ❗ 3. Overcomplicated

👉 Only need:

```awk
ones / NR
```

---

# 🎯 Key Idea

👉
**count 1s → divide by total → compare with 0.5**

---

# ⚠️ Important Points

* No need to track zeros
* `NR` gives total count
* Input always has at least one `1`

---

# 🧠 Memory Trick

👉
**1 count / total → >0.5 → not**

---
