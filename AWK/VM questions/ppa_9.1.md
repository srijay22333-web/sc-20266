# Even-Odd Sum using AWK

---

## 📝 Problem Statement

Write an **AWK script** that:

* Reads a number `n` from **standard input**
* Considers natural numbers from **1 to n (inclusive)**
* Prints:

  * Sum of **odd numbers**
  * Sum of **even numbers**

👉 Each on a **separate line**

---

## 📥 Sample Input

```bash
10
```

---

## 📤 Sample Output

```bash
25
30
```

---

## 💻 Solution (evenodd.awk)

```awk
{
    for (i = 1; i <= $0; i++) {
        if (i % 2 == 0)
            even_sum += i
        else
            odd_sum += i
    }
}

END {
    print odd_sum
    print even_sum
}
```

---

## 🔍 Explanation

### 🔹 `$0`

* Represents the input value `n`

---

### 🔹 Loop

```awk
for (i = 1; i <= $0; i++)
```

* Iterates from **1 to n**

---

### 🔹 Condition

```awk
i % 2 == 0
```

* Checks if number is **even**
* Else → odd

---

### 🔹 Summation

* `even_sum += i` → adds even numbers
* `odd_sum += i` → adds odd numbers

---

### 🔹 END Block

```awk
END {
    print odd_sum
    print even_sum
}
```

* Prints results after processing input

---

## 🎯 Key Concepts

* `$0` → input from stdin
* `%` → modulus operator
* `+=` → summation
* `END` → final output block

---

## ⚠️ Important Points

* Script is written **directly in `.awk` file**
* No `awk '...'` wrapper needed
* No command-line arguments required

---

## 🧠 Memory Trick

👉 **Read n → Loop → Separate → Sum → Print**
