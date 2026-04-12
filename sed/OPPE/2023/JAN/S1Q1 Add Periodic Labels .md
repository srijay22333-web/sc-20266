# Add Periodic Labels using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Adds a new column at the **beginning of each line**
* Inserts `"S1"` for every **3rd line starting from line 1** (1, 4, 7, ...)
* Inserts `"S2"` for all other lines

---

## 📥 Sample Input

```text
Ajay,Chennai,Sales,40
Mona,Mumbai,Sales,60
Rajan,Patna,Buisness,50
Narayanan,Kochi,Sales,30
Anuj,Pune,Sales,70
Mohan,Jaipur,Buisness,70
```

---

## 📤 Sample Output

```text
S1,Ajay,Chennai,Sales,40
S2,Mona,Mumbai,Sales,60
S2,Rajan,Patna,Buisness,50
S1,Narayanan,Kochi,Sales,30
S2,Anuj,Pune,Sales,70
S2,Mohan,Jaipur,Buisness,70
```

---

## 💻 Solution

```bash
sed '
1~3 s/^/S1,/
1~3! s/^/S2,/
'
```

---

## 🔍 Explanation

### 🔹 Step Selection

```bash
1~3
```

* Selects every 3rd line starting from line 1
* Matches lines: 1, 4, 7, 10, ...

---

### 🔹 Negation

```bash
1~3!
```

* Selects all lines **not matching** the above condition

---

### 🔹 Add Prefix

```bash
s/^/S1,/
```

* Adds `"S1,"` at the beginning of selected lines

```bash
s/^/S2,/
```

* Adds `"S2,"` for remaining lines

---

## 🎯 Key Idea

👉 Use **step addressing (`1~3`) + negation (`!`)**

---

## ⚠️ Important Points

* `^` → start of line
* No need for `-n`, `p`, or `d`
* sed prints automatically

---

## 🧠 Memory Trick

👉 **1~3 → S1, rest → S2**
