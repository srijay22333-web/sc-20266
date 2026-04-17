# 📍 Print Digit-Only Lines within Defined Regions (AWK)

---

## 📝 Problem Statement

Write an **AWK script (`script.awk`)** that:

* Processes a file containing multiple lines
* A **region** is defined as:

  * Starts with `# REGION`
  * Ends with `# ENDREGION`

---

### 🎯 Your Task

* Print only those lines:

  * That are **inside the defined region**
  * And contain **only digits (no letters, no spaces)**

---

## 📥 Sample Input

```text id="rg1"
1
a
# REGION
1928374
a long rain
3b
# ENDREGION
a
1
b1234
1
a
# REGION
1928374
a long rain
3b
# ENDREGION
a
1
b1234
```

---

## 📤 Sample Output

```text id="rg2"
1928374
1928374
```

---

## ✅ Solution — script.awk

```awk id="rg3"
#!/usr/bin/gawk -f

/# REGION/, /# ENDREGION/ {
    if ($0 ~ /^[[:digit:]]+$/) {
        print
    }
}
```

---

## 🔍 Explanation

---

### 🔹 Range Pattern

```awk id="rg4"
/# REGION/, /# ENDREGION/
```

* Selects lines:

  * From `# REGION`
  * Until `# ENDREGION`

---

### 🔹 Digit Check

```awk id="rg5"
/^[[:digit:]]+$/
```

* Matches lines containing:

  * Only digits (`0–9`)
  * No letters or spaces

---

### 🔹 Condition

```awk id="rg6"
if ($0 ~ /^[[:digit:]]+$/)
```

* Ensures only valid numeric lines are printed

---

### 🔹 Print

```awk id="rg7"
print
```

* Prints the matching line

---

## 🎯 Key Idea

👉 Combine:

* **range pattern** → select region
* **regex match** → filter content

---

## ⚠️ Important Points

* `# REGION` and `# ENDREGION` lines are included in range

  * But not printed (fail condition)
* `[[:digit:]]` is POSIX-safe
* Works for multiple regions

---

## 🧠 Memory Trick

👉
**Range → Filter → Print**

---

💪 This is a **high-value pattern (range + regex)** — very common in exams 🚀
