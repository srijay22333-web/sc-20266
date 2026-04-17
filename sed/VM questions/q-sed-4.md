# Convert Multi-line CSV Address to Single Line using sed

---

## 📝 Problem Statement

You are given a CSV file through standard input.

Each record contains:

* Name
* Age
* Gender
* City
* Address

👉 The **Address field**:

* Is enclosed in double quotes `"`
* Can span **multiple lines**

---

### 🎯 Task

Write a **sed script (`script.sed`)** that:

* Converts each record into a **single line**
* Replaces **newlines inside the address field** with `, `
* Keeps quotes intact

---

## 📥 Sample Input

```text id="csv1"
Name,Age,Gender,City,Address
Ram,32,M,Ayodhya,"Street 1
Ayodhya
India"
Shyam,45,M,Mathura,"Street 2
Mathura
India"
Gita,23,F,Vrindavan,"Street 3
Vrindavan
India"
```

---

## 📤 Sample Output

```text id="csv2"
Name,Age,Gender,City,Address
Ram,32,M,Ayodhya,"Street 1, Ayodhya, India"
Shyam,45,M,Mathura,"Street 2, Mathura, India"
Gita,23,F,Vrindavan,"Street 3, Vrindavan, India"
```

---

## ✅ Solution — CLI

```bash id="csv3"
sed '
:x
1!{
    /[^"]$/{
        N
        s/\n/, /
        tx
    }
}
' input.txt
```

---

## ✅ Solution — script.sed

```sed id="csv4"
:x
1!{
    /[^"]$/{
        N
        s/\n/, /
        tx
    }
}
```

Run using:

```bash id="csv5"
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Label (Loop Start)

```bash id="csv6"
:x
```

* Defines a label for looping

---

### 🔹 Skip Header Line

```bash id="csv7"
1!
```

* Applies logic to all lines except first

---

### 🔹 Detect Incomplete Address

```bash id="csv8"
/[^"]$/
```

* Matches lines that **do NOT end with `"`**
* Meaning:

  * Address is still continuing

---

### 🔹 Append Next Line

```bash id="csv9"
N
```

* Joins next line into pattern space

---

### 🔹 Replace Newline

```bash id="csv10"
s/\n/, /
```

* Converts newline → `, `

---

### 🔹 Loop Until Address Ends

```bash id="csv11"
tx
```

* If substitution happened → go back to `:x`
* Keeps merging until closing `"` is found

---

## 🎯 Key Idea

👉 **Keep joining lines until closing quote appears**

---

## ⚠️ Important Points

* Works only because:

  * Address always ends with `"`
* `[^"]$` is critical condition
* `tx` ensures looping only when needed

---

## 🧠 Memory Trick

👉 **Not ending with `"` → keep joining → replace newline**

---
