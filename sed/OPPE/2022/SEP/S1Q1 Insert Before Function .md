# Insert Before Function and Remove TODO using sed

---

## 📝 Problem Statement

Write a **sed script (`script.sed`)** that:

* Inserts the line `# FUNCTION DEFINITION` **before every line containing `def`**
* Removes all lines containing `# TODO`

---

## 📥 Sample Input

```text id="fd1"
def a():
    def b():
        pass
    def c():
        pass
    TODO = 9
    return TODO
# ...
```

---

## 📤 Sample Output

```text id="fd2"
# FUNCTION DEFINITION
def a():
# FUNCTION DEFINITION
    def b():
        pass
# FUNCTION DEFINITION
    def c():
        pass
    TODO = 9
    return TODO
# ...
```

---

## ✅ Solution — CLI

```bash id="fd3"
sed '
/# TODO/d
/\<def\>/i # FUNCTION DEFINITION
' input.txt
```

---

## ✅ Solution — script.sed

```sed id="fd4"
/# TODO/d
/\<def\>/i # FUNCTION DEFINITION
```

Run using:

```bash id="fd5"
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Remove TODO Lines

```bash id="fd6"
/# TODO/d
```

* Deletes lines containing `# TODO`

---

### 🔹 Match `def` Keyword

```bash id="fd7"
/\<def\>/
```

* Matches whole word `def`
* Avoids matching words like `define`

---

### 🔹 Insert Line Before Match

```bash id="fd8"
i # FUNCTION DEFINITION
```

* Inserts line **before matched line**
* Pushes original line down

---

## ⚠️ Important Correction (Your Mistake)

❌ You wrote:

```sed id="rt9y39"
/\b.*def\b/
```

👉 Problems:

* `\b` not portable in sed
* `.*def` unnecessary
* Might match wrong patterns

✔️ Correct:

```sed id="7rtt9o"
/\<def\>/
```

---

## 🎯 Key Idea

👉 Match → insert before → delete unwanted lines

---

## ⚠️ Important Points

* `i` inserts before line
* `d` deletes entire line
* Order matters (delete first, then insert)

---

## 🧠 Memory Trick

👉 **def → add label above, TODO → delete**
