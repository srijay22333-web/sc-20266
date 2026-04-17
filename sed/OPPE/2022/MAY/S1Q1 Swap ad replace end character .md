# Swap Fields and Replace End Character using sed

---

## 📝 Problem Statement

Write a **sed script (`script.sed`)** that:

* Swaps the **first and second fields** in each line

  * Fields are separated by `:`
* Replaces `?` with `!` **only if it appears at the end of a line**

---

## 📥 Sample Input

```text id="sf1"
This is field1:And now field2:and now3
Is it field1?:Maybe this is field2:is this field3?
this is not field2:is this not field1?: field3 :field4
and?
```

---

## 📤 Sample Output

```text id="sf2"
And now field2:This is field1:and now3
Maybe this is field2:Is it field1?:is this field3!
is this not field1?:this is not field2: field3 :field4
and!
```

---

## ✅ Solution — CLI (ERE)

```bash id="sf3"
sed -E '
s/^([^:]*):([^:]*)/\2:\1/
s/\?$/!/
' input.txt
```

---

## ✅ Solution — script.sed

```sed id="sf4"
s/^([^:]*):([^:]*)/\2:\1/
s/\?$/!/
```

Run using:

```bash id="sf5"
sed -E -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Swap First Two Fields

```bash id="sf6"
^([^:]*):([^:]*)
```

* `([^:]*)` → matches field (until `:`)
* `\1` → first field
* `\2` → second field

👉 Replacement:

```bash id="sf7"
\2:\1
```

* Swaps first and second fields

---

### 🔹 Replace `?` at End of Line

```bash id="sf8"
\?$
```

* `\?` → literal `?`
* `$` → end of line

👉 Replacement:

```bash id="sf9"
!
```

---

## 🎯 Key Idea

👉 Capture fields → swap → fix line ending

---

## ⚠️ Important Points

* Only first two fields are swapped
* `\?` must be escaped
* `$` ensures only end-of-line replacement

---

## 🧠 Memory Trick

👉 **field1:field2 → field2:field1 + ? → ! at end**
