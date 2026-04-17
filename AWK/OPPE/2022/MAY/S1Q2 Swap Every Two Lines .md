# 🔄 Swap Every Two Lines using AWK

---

## 📝 Problem Statement

Write an **AWK script (`script.awk`)** that:

* Reads a file where:

  * Keys and values should appear alternately
  * But currently they are **swapped (wrong order)**

* The file contains:

  * Value on odd lines
  * Key on even lines

---

### 🎯 Your Task

* Swap every **two consecutive lines**
* Correct the order:

  * Key first
  * Then value

---

## 📥 Sample Input

```text id="sw1"
value1
key1
value2
key2
value3
key3
```

---

## 📤 Sample Output

```text id="sw2"
key1
value1
key2
value2
key3
value3
```

---

## ✅ Solution — script.awk

```awk id="sw3"
#!/usr/bin/gawk -f

NR % 2 == 1 {
    valueLine = $0
}

NR % 2 == 0 {
    print $0
    print valueLine
}
```

---

## 🔍 Explanation

### 🔹 NR (Record Number)

```awk id="sw4"
NR % 2
```

* Odd line → `NR % 2 == 1`
* Even line → `NR % 2 == 0`

---

### 🔹 Store Odd Line

```awk id="sw5"
valueLine = $0
```

* Stores value line

---

### 🔹 Print in Swapped Order

```awk id="sw6"
print $0
print valueLine
```

* First prints key (even line)
* Then prints stored value

---

## 🎯 Key Idea

👉 Use **NR % 2 to group lines in pairs**

---

## ⚠️ Important Points

* File always has **even number of lines**
* Order must be strictly swapped
* No extra storage needed beyond one variable

---

## 🧠 Memory Trick

👉 **Odd → store, Even → print + swap**

