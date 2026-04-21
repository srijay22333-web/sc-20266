

# Convert Quoted CSV to TSV using sed

---

## 📝 Problem Statement

You are given a **CSV file** where:

* Each field is enclosed in **double quotes `"`**
* Fields are separated by commas `,`

---

### 🎯 Task

Write a **sed script (`script.sed`)** that:

* Converts CSV → TSV (Tab Separated Values)
* Removes all **double quotes**
* Replaces **commas between fields** with **tab (`\t`)**
* Prints output to standard output

---

## 📥 Sample Input

```text id="csvt1"
"field1","field2","field3"
"alpha,beta","1453.23","-123.2"
"mu,phi,nu","1 1342.23","-12.2"
```

---

## 📤 Sample Output

```text id="csvt2"
field1	field2	field3
alpha,beta	1453.23	-123.2
mu,phi,nu	1 1342.23	-12.2
```

---

## 💻 Solution (Script Format)

```sed id="csvt3"
s/"//g
s/,/\t/g
```

Run using:

```bash id="csvt4"
sed -f script.sed input.csv
```

---

## 🔍 Explanation

### 🔹 Remove Quotes

```sed id="csvt5"
s/"//g
```

* Removes all double quotes
* Leaves only raw field content

---

### 🔹 Replace Comma with Tab

```sed id="csvt6"
s/,/\t/g
```

* Converts field separator `,` → tab

---

## 🎯 Key Idea

👉 **Remove quotes → replace separators**

---

## ⚠️ Important Note

* Works because:

  * Commas inside fields are protected by quotes
  * After removing quotes, they remain part of data

---

## 🧠 Memory Trick

👉 **"remove quotes → comma → tab"**

---

## 🚀 One-Line Version (CLI)

```bash id="csvt7"
sed 's/"//g; s/,/\t/g'
```
