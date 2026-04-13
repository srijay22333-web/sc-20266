# Swap 4th Column to First using sed (ERE)

---

## 📝 Problem Statement

A file `mydata.csv` is present in the current working directory.

Write a **sed script** that:

* Swaps the **fourth column** with the **first column**
* Keeps the other columns in the same order

---

## 📥 Sample Input

```text id="sw1"
grep,219,10.95,47961
ls,208,10.4,43264
echo,195,9.75,38025
ps,92,4.6,8464
sed,82,4.1,6724
cd,80,4,6400
vi,77,3.85,5929
clear,72,3.6,5184
awk,72,3.6,5184
man,67,3.35,4489
```

---

## 📤 Sample Output

```text id="sw2"
47961,219,10.95,grep
43264,208,10.4,ls
38025,195,9.75,echo
8464,92,4.6,ps
6724,82,4.1,sed
6400,80,4,cd
5929,77,3.85,vi
5184,72,3.6,clear
5184,72,3.6,awk
4489,67,3.35,man
```

---

## 💻 Solution (Command Line - ERE)

```bash id="sw3"
sed -E 's/^([^,]+),([^,]+),([^,]+),([^,]+)/\4,\2,\3,\1/' mydata.csv
```

---

## 💻 Solution (script.sed - ERE)

```sed id="sw4"
s/^([^,]+),([^,]+),([^,]+),([^,]+)/\4,\2,\3,\1/
```

Run using:

```bash id="sw5"
sed -E -f script.sed mydata.csv
```

---

## 🔍 Explanation

### 🔹 Capture Columns

```bash id="sw6"
([^,]+),([^,]+),([^,]+),([^,]+)
```

* `([^,]+)` → matches one column (anything except `,`)
* Total 4 groups:

  * `\1` → column 1
  * `\2` → column 2
  * `\3` → column 3
  * `\4` → column 4

---

### 🔹 Reorder Columns

```bash id="sw7"
\4,\2,\3,\1
```

* Moves:

  * column 4 → first
  * column 1 → last

---

## 🎯 Key Idea

👉 Use **grouping + backreferences** to rearrange columns

---

## ⚠️ Important Points

* Use `-E` for cleaner grouping `()`
* `[^,]+` ensures correct column separation
* Works for any 4-column CSV format

---

## 🧠 Memory Trick

👉 **Capture 4 → print 4,2,3,1**
