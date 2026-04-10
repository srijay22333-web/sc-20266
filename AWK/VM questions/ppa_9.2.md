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

## 💻 Solution (repeat.awk)

```awk id="p4v0u1"
{
    for (i = 1; i < NF; i++) {
        w1 = tolower($i)
        w2 = tolower($(i+1))

        if (w1 == w2 && !seen[w1]) {
            print w1
            seen[w1] = 1
        }
    }
}
```

---

## 🔍 Explanation

### 🔹 Loop Through Words

```awk id="u6q1p6"
for (i = 1; i < NF; i++)
```

* Iterates till second-last word
* Allows comparison with next word

---

### 🔹 Convert to Lowercase

```awk id="m1k8jh"
w1 = tolower($i)
w2 = tolower($(i+1))
```

* Ensures case-insensitive matching

---

### 🔹 Check Duplicate

```awk id="s9s7we"
if (w1 == w2 && !seen[w1])
```

* Checks consecutive duplicates
* Avoids repeated printing

---

### 🔹 Store Printed Words

```awk id="1zx2ya"
seen[w1] = 1
```

* Marks word as already printed

---

## 🎯 Key Idea

👉 Compare **current word** with **next word**

---

## ⚠️ Important Points

* Use `$(i+1)` (NOT `$i+1`)
* Works only for **consecutive duplicates**
* Output order is preserved

---

## 🧠 Memory Trick

👉 **Current == Next → Print once → Mark seen**
