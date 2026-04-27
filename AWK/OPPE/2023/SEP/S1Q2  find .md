# 📘 Find Tagged Comments in Files (AWK)

---

## 📝 Problem Statement

Programmers often tag comments using keywords like:

```text id="kw1"
TODO, REVIEW, NOTE, HACK, FIXME, DEPRECATED
```

---

### 🎯 Your Task

Write an AWK script that:

* Searches for lines containing any of these keywords
* Prints:

```text id="kw2"
<filename>:<line_number>
```

---

## 📥 Sample Input (multiple files)

```text id="kw3"
file1.c
file2.c
```

👉 Assume files contain lines with keywords

---

## 📤 Sample Output

```text id="kw4"
file1.c:3
file1.c:10
file2.c:5
```

---

# 💻 Solution (script.awk)

```awk
#!/usr/bin/gawk -f

/TODO|REVIEW|NOTE|HACK|FIXME|DEPRECATED/ {
    print FILENAME ":" FNR
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 Pattern Matching

```awk
/TODO|REVIEW|NOTE|.../
```

👉 Matches any of the keywords

---

## 🔹 File Name

```awk
FILENAME
```

👉 Current file name

---

## 🔹 Line Number

```awk
FNR
```

👉 Line number **within that file**

---

## 🔹 Print Format

```awk
print FILENAME ":" FNR
```

---

# 🎯 Key Idea

👉
**pattern match → print file + line**

---

# ⚠️ Important Points

* `FNR` resets for each file
* Use `|` for multiple keywords
* Works for multiple files:

```bash
gawk -f script.awk file1 file2
```

---

# 🧠 Memory Trick

👉
**keyword → file:line**

---


