# Add Serial Number to CSV using sed

---

## 📝 Problem Statement

Write a **sed-based solution** that:

* Adds a **serial number as the first column** to each line in `data.csv`
* Serial number should start from **1**
* Modify the file **in-place** (overwrite the file)

---

## 📥 Sample Input

```text id="sn1"
Ajay,Chennai,Sales,40
Mona,Mumbai,Sales,60
Rajan,Patna,Buisness,50
```

---

## 📤 Sample Output

```text id="sn2"
1,Ajay,Chennai,Sales,40
2,Mona,Mumbai,Sales,60
3,Rajan,Patna,Buisness,50
```

---

## 💻 Solution (Command Line)

```bash id="sn3"
sed -i '=' data.csv
sed -i 'N; s/\n/,/' data.csv
```

---

## 💻 Solution (script.sed)

```sed id="sn4"
=
N
s/\n/,/
```

Run using:

```bash id="sn5"
sed -i -f script.sed data.csv
```

---

## 🔍 Explanation

### 🔹 Print Line Number

```bash id="sn6"
=
```

* Prints line number before each line

---

### 🔹 Append Next Line

```bash id="sn7"
N
```

* Combines:

  ```
  number\nline
  ```

---

### 🔹 Replace Newline

```bash id="sn8"
s/\n/,/
```

* Converts:

  ```
  number\nline → number,line
  ```

---

## 🎯 Key Idea

👉 Use:

* `=` → generate line numbers
* `N` → merge lines
* `s/\n/,/` → format as CSV

---

## ⚠️ Important Points

* `-i` → modifies file in-place
* Order of commands matters
* Works line-by-line sequentially

---

## 🧠 Memory Trick

👉 **Print number → join → replace newline → done**
