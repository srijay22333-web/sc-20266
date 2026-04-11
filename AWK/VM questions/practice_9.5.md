# 🔁 Remove Adjacent Duplicate Lines (uniq using AWK)

---

## 📝 Problem Statement

The Linux `uniq` command removes **adjacent duplicate lines** from a file.

Write an **AWK script** that:

* Reads input from standard input
* Removes **only consecutive duplicate lines**
* Prints the result

---

## 📥 Sample Input

```text id="uq1"
1
1
1
1
1
1
1
1
1
1
1
2
```

---

## 📤 Sample Output

```text id="uq2"
1
2
```

---

## 💻 Solution (script.awk)

```awk id="uniqawk"
#!/usr/bin/gawk -f

{
    if ($0 != prev) {
        print $0
    }
    prev = $0
}
```

---

## 🔍 Explanation

---

### 🔹 `$0`

```awk id="uq3"
$0
```

* Represents the **entire current line**

---

### 🔹 `prev`

```awk id="uq4"
prev
```

* Stores the **previous line**
* Initially empty

---

### 🔹 Condition

```awk id="uq5"
if ($0 != prev)
```

* Prints line only if it is **different from previous line**

---

### 🔹 Update Previous Value

```awk id="uq6"
prev = $0
```

* Moves forward for next comparison

---

## 🎯 Key Idea

👉 Compare **current line with previous line**

---

## ⚠️ Important Points

* This removes only **adjacent duplicates**
* Does NOT remove all duplicates globally
* Works like Linux `uniq`

---

## 🧠 Memory Trick

👉 **uniq = current ≠ previous**

---

## 🔄 Comparison with Global Unique

### ❌ Wrong for this problem:

```awk id="uq7"
!seen[$0]++
```

👉 Removes all duplicates globally

---

### ✅ Correct for this problem:

```awk id="uq8"
$0 != prev
```

👉 Removes only consecutive duplicates

---

## 🚀 Final Insight

This problem teaches:

* Difference between:

  * **global duplicate removal**
  * **adjacent duplicate removal**
* State tracking using variables

---
