# 📘 Find Maximum Value per Name (AWK)

---

## 📝 Problem Statement

You are given input where each line contains:

```text id="m1"
Name Value
```

---

### 🎯 Your Task

* For each name:

  * Find the **maximum value**
* Print:

```text id="m2"
Name MaxValue
```

---

## 📥 Sample Input

```text id="m3"
Alice 10
Alice 25
Bob 20
Alice 15
Bob 30
```

---

## 📤 Sample Output

```text id="m4"
Alice 25
Bob 30
```

---

# 💻 Solution (script.awk)

```awk id="m5"
#!/usr/bin/gawk -f

{
    if (!($1 in max) || $2 > max[$1])
        max[$1] = $2
}

END {
    for (name in max)
        print name, max[name]
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 Check if first time OR larger value

```awk id="m6"
!($1 in max)
```

👉 First time seeing name

---

```awk id="m7"
$2 > max[$1]
```

👉 New value is bigger

---

## 🔹 Update max

```awk id="m8"
max[$1] = $2
```

---

# 🎯 Key Idea

👉
**if new OR bigger → replace**

---

# ⚠️ Important Case (NEGATIVE VALUES)

---

## 📥 Input

```text id="m9"
Alice -10
Alice -25
Bob -20
Alice -5
Bob -30
```

---

## 📤 Output

```text id="m10"
Alice -5
Bob -20
```

---

### ✅ Why your code works for negatives

Because of:

```awk id="m11"
!($1 in max)
```

👉 First value initializes correctly
👉 Then comparisons work normally

---

# ❌ Wrong Approach (Common Mistake)

```awk id="m12"
max[$1] = 0
```

👉 ❌ fails for negative values

---

# 🧠 Memory Trick

👉
**first OR bigger → update**

---

# 🚀 Final Insight

This is a **max-tracking pattern**:

```text id="m13"
initialize → compare → replace
```
