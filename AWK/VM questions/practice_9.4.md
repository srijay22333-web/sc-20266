# 🔢 Count Number of Lines using AWK

---

## 📝 Problem Statement

Write an **AWK script** that:

* Reads input from a file (via standard input)
* Counts the **total number of lines**
* Prints the final count

---

## 📥 Sample Input

```text
a
b
```

---

## 📤 Sample Output

```text
2
```

---

## 💻 Solution (script.awk)

```awk id="linecount"
#!/usr/bin/gawk -f

END {
    print NR
}
```

---

## 🔍 Explanation

---

### 🔹 Built-in Variable `NR`

```awk id="nruse"
NR
```

* `NR` = Number of Records (lines)
* Automatically increases for each line read

---

### 🔹 END Block

```awk id="endblock"
END {
    print NR
}
```

* Runs **after all input is processed**
* Prints the **final line count**

---

## 🎯 Key Idea

👉 Let AWK count lines automatically using `NR`

---

## ⚠️ Important Points

* Do NOT print inside `{}` block → prints every line count ❌
* Use `END` → prints only final result ✔
* No need for manual counter

---

## 🧠 Memory Trick

👉 **NR grows → END shows final value**

---
