# 🔁 Find Repeated Words using AWK

---

## 📝 Problem Statement

Write an **AWK script** that:

* Reads a file `myfile.txt`
* Detects **consecutive duplicate words** (like `the the`)
* Prints:

  * Only the **repeated word**
  * **One per line**
  * In the **order of occurrence**
  * Without printing duplicates more than once

---

## 📥 Sample Input

```text
Long years ago, we made a tryst with destiny; and now the the time comes when we shall redeem our pledge, not not wholly or in full measure, but very very substantially.
At the stroke of the midnight hour, when the world sleeps, India will will awake to life and freedom.
```

---

## 📤 Sample Output

```text
the
not
very
will
```

---

## 💻 Solution (repeat.awk)

```awk
{
    for (i = 1; i <= NF; i++) {
        word = tolower($i)

        if (word == prev && !seen[word]) {
            print word
            seen[word] = 1
        }

        prev = word
    }
}
```

---

## 🔍 Explanation

### 🔹 Loop Through Words

```awk
for (i = 1; i <= NF; i++)
```

* Processes each **word (field)** in the line

---

### 🔹 `tolower($i)`

* Converts words to lowercase
  👉 Ensures:

```
"The the" → treated as duplicate
```

---

### 🔹 `word == prev`

* Checks if current word is same as **previous word**

---

### 🔹 `!seen[word]`

* Ensures word is printed **only once**

---

### 🔹 `seen[word] = 1`

* Marks word as already printed

---

## 🎯 Key Concepts

* `NF` → number of words in a line
* `tolower()` → normalize case
* `prev` → stores previous word
* Associative array `seen[]` → avoids duplicates

---

## ⚠️ Important Points

* Only **consecutive duplicates** are considered
* Case-insensitive comparison
* Output preserves **first occurrence order**

---

## 🧠 Memory Trick

👉 **Compare with previous → print once → mark as seen**
