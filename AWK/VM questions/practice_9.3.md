
# 📘 Union of Two Files (Distinct Lines)

---

# Problem Statement

You are given two files:

* `file1`
* `file2`

Each file contains multiple lines.

---

### 🎯 Your Task

* Combine both files
* Print only **distinct (unique) lines**
* Maintain order:

  * First print lines from `file1`
  * Then from `file2` (only if not already printed)

---

# Sample Input

### file1

```text id="u1"
1
2
3
```

### file2

```text id="u2"
1
2
```

---

# Sample Output

```text id="u3"
1
2
3
```

---

# 🔹 Solution (union.awk)

```bash id="u4"
#!/usr/bin/gawk -f

{
    if (!seen[$0]) {
        seen[$0] = 1
        print $0
    }
}
```

---

# 🔍 Explanation (Simple Way)

---

## 🔹 `$0`

👉 Entire line
Example:

```text id="u5"
1
2
3
```

---

## 🔹 `seen[$0]`

👉 Keeps track of lines already printed

---

## 🔹 Condition

```awk id="u6"
if (!seen[$0])
```

👉 Means:

* If line is NOT seen before

---

## 🔹 Store + Print

```awk id="u7"
seen[$0] = 1
print $0
```

👉 Mark as seen
👉 Print it

---

# 🧠 How It Works Step-by-Step

---

### Processing file1

```text id="u8"
1 → print  
2 → print  
3 → print  
```

---

### Processing file2

```text id="u9"
1 → already seen → skip  
2 → already seen → skip  
```

---

# ⚡ Key Concept

👉 This is a **set (unique values) problem**

---

# 🧠 Memory Trick

👉 `seen[x]` → “Have I printed this before?”

---
