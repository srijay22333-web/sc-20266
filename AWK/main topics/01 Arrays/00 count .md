# 📘 Count Frequency of Words (AWK)

---

## 📝 Problem Statement

You are given a list of words, one per line.

---

### 🎯 Your Task

* Count how many times each word appears
* Print:

```text id="cw1"
word count
```

---

## 📥 Sample Input

```text id="cw2"
apple
banana
apple
orange
banana
apple
```

---

## 📤 Sample Output

```text id="cw3"
apple 3
banana 2
orange 1
```

---

# 💻 Solution (script.awk)

```awk id="cw4"
#!/usr/bin/gawk -f

{
    count[$1]++
}

END {
    for (w in count)
        print w, count[w]
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 Count Words

```awk id="cw5"
count[$1]++
```

👉 `$1` = word
👉 increments frequency

---

## 🔹 Print Result

```awk id="cw6"
for (w in count)
```

👉 loop through all words

---

# 🎯 Key Idea

👉
**word → key → count++**

---

# ⚠️ Important Points

* Output order may vary (AWK arrays unordered)
* Works even if words repeat many times

---

# 🧠 Memory Trick

👉
**key → count++ → print**

---
