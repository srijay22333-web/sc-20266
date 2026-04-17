# Extract Multi-line String Content using sed

---

## 📝 Problem Statement

Write a **sed script (`script.sed`)** that:

* Prints only the content inside Python multi-line strings
* Multi-line strings:

  * Start with `"""`
  * End with `"""`
* Conditions:

  * There is `=` before starting `"""` (with optional spaces)
  * No text exists on same line as `"""`

---

## 📥 Sample Input 1

```text id="ml1"
a = """
abcd
"""
```

---

## 📤 Sample Output 1

```text id="ml2"
abcd
```

---

## 📥 Sample Input 2

```text id="ml3"
multi="""
a
b
c t
d 5
"""
```

---

## 📤 Sample Output 2

```text id="ml4"
a
b
c t
d 5
```

---

## 📥 Sample Input 3

```text id="ml5"
multi="""
a
b

c t
d 5
"""
```

---

## 📤 Sample Output 3

```text id="ml6"
a
b

c t
d 5
```

---

## 📥 Sample Input 4

```text id="ml7"
a = """
0
"""
```

---

## 📤 Sample Output 4

```text id="ml8"
0
```

---

## 📥 Sample Input 5

```text id="ml9"
ui_2="""
9876432345678
"""
```

---

## 📤 Sample Output 5

```text id="ml10"
9876432345678
```

---

## 📥 Sample Input 6

```text id="ml11"
var= """

-----

"""
```

---

## 📤 Sample Output 6

```text id="ml12"
-----
```

---

## ✅ Solution — CLI

```bash id="ml13"
sed -n '/"""/,/"""/{
/"""/!p
}' myscript.py
```

---

## ✅ Solution — script.sed

```sed id="ml14"
/"""/,/"""/{
/"""/!p
}
```

Run using:

```bash id="ml15"
sed -n -f script.sed myscript.py
```

---

## 🔍 Explanation

### 🔹 Select Range Between `"""`

```bash id="ml16"
/"""/,/"""/
```

* Selects lines from opening `"""` to closing `"""`

---

### 🔹 Exclude Boundary Lines

```bash id="ml17"
/"""/!p
```

* Prints only lines that do NOT contain `"""`
* Removes opening and closing markers

---

### 🔹 Suppress Default Output

```bash id="ml18"
-n
```

* Prevents printing unwanted lines

---

## 🎯 Key Idea

👉 Use **range selection + conditional print**

---

## ⚠️ Important Points

* Works for multi-line blocks
* Handles empty lines inside block
* Assumes clean `"""` format (as per problem)

---

## 🧠 Memory Trick

👉 **Select block → skip markers → print inside**

---

## 🚀 Your Level

You just used:

* Range addressing ✔️
* Negation (`!`) ✔️
* Controlled printing ✔️

👉 This is **core sed multi-line extraction pattern (very important)** 💯
