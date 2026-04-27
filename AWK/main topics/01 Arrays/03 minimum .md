# 📘 Find Minimum Value per Name (AWK)

---

## 📝 Problem Statement

You are given input where each line contains:

```text
Name Value
```

---

### 🎯 Your Task

* For each name:

  * Find the **minimum value**
* Print:

```text
Name MinValue
```

---

## 📥 Sample Input

```text
Alice 10
Alice 25
Bob 20
Alice 15
Bob 30
```

---

## 📤 Sample Output

```text
Alice 10
Bob 20
```

---

# 💻 Solution (script.awk)

```awk
#!/usr/bin/gawk -f

BEGIN {
	FS = " "
}

{
	name = $1
    value = $2
    if (!(name in min) || value < min[name])
        min[name] = $2
}

END {
    for (name in min)
        print name, min[name]
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 First time OR smaller value

```awk
!($1 in min)
```

👉 First occurrence initializes value

```awk
$2 < min[$1]
```

👉 New value is smaller

---

## 🔹 Update minimum

```awk
min[$1] = $2
```

---

# 🎯 Key Idea

👉
**if new OR smaller → replace**

---

# ⚠️ Important Case (NEGATIVE VALUES)

---

## 📥 Input

```text
Alice -10
Alice -25
Bob -20
Alice -5
Bob -30
```

---

## 📤 Output

```text
Alice -25
Bob -30
```

---

### ✅ Why this works

* First value initializes
* Then compares correctly even for negatives

---

# ❌ Wrong Approach (Common Mistake)

```awk
min[$1] = 0
```

👉 ❌ fails for positive-only or mixed values

---

# 🧠 Memory Trick

👉
**first OR smaller → update**


Answer that → you’ve mastered **min problems in AWK (including negatives)** 🚀
