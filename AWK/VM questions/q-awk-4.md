## 🔄 Restore Corrupted Records

---

# Problem Statement

You are working on a data file for a Machine Learning Project, but the data might be corrupted during transit.

The file is supposed to have:

* A **field delimiter** (like `,`)
* Proper **records (rows)** with fixed number of columns

However:

* The **record separators are lost**
* Everything is merged into a **single line**
* Only field delimiters remain

---

### 🎯 Your Task

Reconstruct the original records by:

* Grouping every **N fields into one record**
* Using the given delimiter
* Printing each reconstructed record on a new line

---

### 🧾 Given

* `N` → number of fields per record
* `DELIMITER` → field separator

---

# Sample Input

```text id="rc1"
1,2,3,4,5,6,7,8,9,10,11,12
```

```
N = 3  
DELIMITER = ','
```

---

# Sample Output

```text id="rc2"
1,2,3
4,5,6
7,8,9
10,11,12
```

---

# Solution

```bash id="rc3"
awk -v N="$N" -v FS="$DELIMITER" -v OFS="$DELIMITER" '
{
    for (i = 1; i <= NF; i++) {
        printf "%s", $i

        if (i % N == 0)
            printf "\n"
        else
            printf OFS
    }
}
'
```

---

# Explanation

---

### 🔹 Field Splitting

* `FS="$DELIMITER"` → splits input into fields
* Example:

  ```
  1,2,3,4 → $1=1, $2=2, $3=3, $4=4
  ```

---

### 🔹 Loop Through Fields

```bash id="rc4"
for (i = 1; i <= NF; i++)
```

👉 Iterates through all fields

---

### 🔹 Printing Fields

```bash id="rc5"
printf "%s", $i
```

👉 Prints each field **without newline**

---

### 🔹 Grouping Logic

```bash id="rc6"
if (i % N == 0)
```

👉 Every N fields → new record

---

### 🔹 Output Formatting

```bash id="rc7"
printf "\n"
```

👉 Moves to next line after N fields

```bash id="rc8"
printf OFS
```

👉 Prints delimiter between fields

---

## 🧠 Key Concept

👉 Use **modulus (`%`)** to group fields into records

👉 Every `N` fields → create a new row

---

## 🔥 Memory Trick

👉 `i % N == 0` → “end of one record”

---
