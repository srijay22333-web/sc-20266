# 📘 Count Number of Fields with Multiple Delimiters

---

## 📝 Problem Statement

Given a file `data.txt`, count the number of fields in each line using:

* `:` and `-` as separators

---

## 🎯 Task

* Split each line using `:` and `-`
* Print the number of fields (`NF`) for each line

---

## 📥 Sample Input

```text id="6bf2ey"
John:25,Engineer
Alice-30,Doctor
Bob:22-Artist,Canada
Eve:28-Designer
```

---

## 📤 Sample Output

```text id="v1z7xh"
2
2
3
3
```

---

## ✅ Solution — script.awk

```awk id="72uqp3"
BEGIN {
    FS = "[:-]"
}

{
    print NF
}
```

---

## ▶️ Run

```bash id="xrl2r8"
awk -f script.awk data.txt
```

---

## 🔍 Explanation

* `FS = "[:-]"` → field separator is either `:` or `-`
* `NF` → number of fields in the current line
* `print NF` → prints field count per line

---

## 🧠 Visual Understanding

### ▶ Line 1

```text id="k19r2i"
John:25,Engineer
```

Split using `:`:

```text id="3lmt0p"
John | 25,Engineer
```

👉 Fields = **2**

---

### ▶ Line 2

```text id="4u0qhs"
Alice-30,Doctor
```

Split using `-`:

```text id="0j2kxr"
Alice | 30,Doctor
```

👉 Fields = **2**

---

### ▶ Line 3

```text id="yqb9r3"
Bob:22-Artist,Canada
```

Split using both:

```text id="cccz1o"
Bob | 22 | Artist,Canada
```

👉 Fields = **3**

---

### ▶ Line 4

```text id="pmghsf"
Eve:28-Designer
```

Split:

```text id="c6o3rd"
Eve | 28 | Designer
```

👉 Fields = **3**

---

## 🔥 Key Concept

```text id="6m9l5k"
FS = "[:-]" → multiple delimiters using regex
```
