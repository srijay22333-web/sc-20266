# 📘 Sum Values per Key (AWK)

---

## 📝 Problem Statement

You are given input where each line contains:

```text id="s1"
Name Value
```

---

### 🎯 Your Task

* For each name:

  * Add all values
* Print:

```text id="s2"
Name Total
```

---

## 📥 Sample Input

```text id="s3"
Alice 10
Bob 20
Alice 15
Bob 5
Alice 5
```

---

## 📤 Sample Output

```text id="s4"
Alice 30
Bob 25
```

---

# 💻 Solution (script.awk)

```awk id="s5"
#!/usr/bin/gawk -f

{
    sum[$1] += $2
}

END {
    for (name in sum)
        print name, sum[name]
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 Add Values

```awk id="s6"
sum[$1] += $2
```

👉 `$1` = name (key)
👉 `$2` = value

---

## 🔹 Print Result

```awk id="s7"
for (name in sum)
```

👉 loops through all names

---

# 🎯 Key Idea

👉
**group by name → sum values**

---

# ⚠️ Important Points

* Order may vary (AWK arrays unordered)
* Works for any number of entries

---

# 🧠 Memory Trick

👉
**key → add → print**

---
