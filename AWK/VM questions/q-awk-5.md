## 🔄 Convert Windows Line Endings to Unix

---

# Problem Statement

You are working with a file created in the Windows operating system.

Windows uses:

```
\r\n
```

as a line ending, whereas most Unix/Linux systems use:

```
\n
```

---

### 🎯 Your Task

Convert the file into Unix format by:

* Replacing Windows line endings (`\r\n`)
* With Unix line endings (`\n`)
* Without changing the actual content

---

### 📥 Input

* Data is provided via **standard input**
* Each line may end with `\r\n`

---

# Sample Input

```text id="win1"
hexsub\r\n
ravindranath\r\n
paraform\r\n
laich\r\n
trammelhead\r\n
constituting\r\n
periclitate\r\n
viertel\r\n
fotive\r\n
nonbelief\r\n
```

---

# Sample Output

```text id="win2"
hexsub
ravindranath
paraform
laich
trammelhead
constituting
periclitate
viertel
fotive
nonbelief
```

---

# Solution

```bash id="win3"
#!/usr/bin/gawk -f

BEGIN {
    RS = "\r\n"
    ORS = "\n"
}

1
```

---

# Explanation

---

### 🔹 RS (Record Separator)

```bash id="win4"
RS = "\r\n"
```

👉 Tells AWK:

* Each record ends with **Windows newline**
* So AWK correctly reads each line

---

### 🔹 ORS (Output Record Separator)

```bash id="win5"
ORS = "\n"
```

👉 Tells AWK:

* Print lines using **Unix newline**

---

### 🔹 Pattern `1`

```bash id="win6"
1
```

👉 This is a shortcut for:

```awk
{ print $0 }
```

✔ Always true
✔ Prints every line

---

## 🧠 Key Concept

👉 Change:

* **RS** → how input is read
* **ORS** → how output is written

---

## 🔥 Memory Trick

👉 **R = Read, O = Output**

* RS → how to read records
* ORS → how to print records

---

