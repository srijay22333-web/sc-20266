## Unfold Lines (Script Format with Loop)

---

# Problem Statement

You are given multiple lines of text as input. Some lines are **folded** using a backslash `\` at the end.

Your task is to **unfold the lines** by joining lines where a backslash appears at the end.

The transformation must:

* Remove the backslash at the end of a line
* Join it with the next line
* Keep other backslashes unchanged

---

# Sample Input

```text id="u10"
Hello\, this is \
a text file.
It has some lines \
folded using a backslash \
at the end of the line.
```

---

# Sample Output

```text id="u11"
Hello\, this is a text file.
It has some lines folded using a backslash at the end of the line.
```

---

# Solution (Script Format)

```sed id="u12"
:a
/\\$/ {
    N
    s/\\\n//
    ba
}
```

---

# Explanation

### 🔹 `:a`

* Defines a **label** named `a`
* Used for looping

---

### 🔹 `/\\$/`

* Matches lines that **end with backslash `\`**
* `\\` → literal backslash
* `$` → end of line

---

### 🔹 `{ ... }`

* Group of commands executed when condition matches

---

### 🔹 `N`

* Appends the **next line** to current pattern space
* Now both lines are processed together

---

### 🔹 `s/\\\n//`

* Removes:

  * backslash `\`
  * newline `\n`
* This effectively **joins two lines**

---

### 🔹 `ba`

* Branch (jump) back to label `a`
* Repeats process for multiple folded lines

---

## 🧠 Key Concept

👉 This uses:

* **Looping (`:a`, `ba`)**
* **Multi-line processing (`N`)**
* **Pattern matching (`\\$`)**

---

## ⚠️ Important Note

👉 This script format is equivalent to:

```bash id="u13"
sed ':a; /\\$/ { N; s/\\\n//; ba }'
```

✔️ One-line → used in HackerRank
✔️ Multi-line → used in **script files / OPPE**

---
