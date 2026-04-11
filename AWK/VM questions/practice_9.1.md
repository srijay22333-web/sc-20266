# 🔢 Find Nth Fibonacci Number using AWK

---

## 📝 Problem Statement

Write an **AWK script** that:

* Takes `n` as a **command-line argument**
* Prints the **nth Fibonacci number**
* Sequence:

  * `1, 1, 2, 3, 5, ...`
  * First two values are `1` and `1`

---

## 📥 Sample Input

```bash
awk -f file.awk 5
```

---

## 📤 Sample Output

```text
5
```

---

## 💻 Solution (file.awk)

```awk id="fibok"
BEGIN {

    n = ARGV[1]
    a = 1
    b = 1

    if (n <= 2){
        print 1
        exit
    }

    for (i = 3; i <= n; i++) {
        c = a + b
        a = b
        b = c
    }

    print b
}
```

---

## 🔍 Explanation

### 🔹 Read Input

```awk id="readn"
n = ARGV[1]
```

* Gets `n` from command line argument

---

### 🔹 Initialize Values

```awk id="init2"
a = 1
b = 1
```

* First two Fibonacci numbers

---

### 🔹 Handle Base Case

```awk id="base2"
if (n <= 2)
```

* First two terms are always `1`

---

### 🔹 Generate Fibonacci

```awk id="loop2"
for (i = 3; i <= n; i++)
```

* Build sequence iteratively

---

## 🎯 Key Idea

👉 Use **ARGV[1] + iterative addition**

---

## ⚠️ Important Points

* Do not use `-v n=...` here
* Input comes from `ARGV[1]`
* Works without reading any file

---

## 🧠 Memory Trick

👉 **Take n → loop → keep adding last two**
