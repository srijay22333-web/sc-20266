# Replace Days and Modify Exclamation Marks using sed

---

## 📝 Problem Statement

Write a **sed script (`script.sed`)** that:

* Replaces **3-letter day abbreviations** (case-insensitive) with numbers:

| Day | Replace |
| --- | ------- |
| sun | 1       |
| mon | 2       |
| tue | 3       |
| wed | 4       |
| thu | 5       |
| fri | 6       |
| sat | 7       |

* Replaces the **first and third occurrences** of `!` with `.` in each line

---

## 📥 Sample Input

```text id="day1"
first exclamation! now second! now third! and now fourth!
first! second! third! fourth!
sun! mon! tue! wed! sat!
fri
thu
all weeks done!
all weeks! yes ! sunday
done thursday
```

---

## 📤 Sample Output

```text id="day2"
first exclamation. now second! now third. and now fourth!
first. second! third. fourth!
1. 2! 3. 4! 7!
6
5
all weeks done.
all weeks. yes ! 1day
done 5rsday
```

---

## ✅ Solution — CLI

```bash id="day3"
sed '
s/sun/1/gi
s/mon/2/gi
s/tue/3/gi
s/wed/4/gi
s/thu/5/gi
s/fri/6/gi
s/sat/7/gi

s/!/./1
s/!/./3
' input.txt
```

---

## ✅ Solution — script.sed

```sed id="day4"
s/sun/1/gi
s/mon/2/gi
s/tue/3/gi
s/wed/4/gi
s/thu/5/gi
s/fri/6/gi
s/sat/7/gi

s/!/./1
s/!/./3
```

Run using:

```bash id="day5"
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Replace Day Abbreviations

```bash id="day6"
s/sun/1/gi
```

* `g` → replace all occurrences
* `i` → case-insensitive

👉 Works for:

* `sun`, `Sun`, `SUN`, etc.

---

### 🔹 Replace 1st Occurrence of `!`

```bash id="day7"
s/!/./1
```

* Replaces only the **first `!`**

---

### 🔹 Replace 3rd Occurrence of `!`

```bash id="day8"
s/!/./3
```

* Replaces only the **third `!`**

---

## 🎯 Key Idea

👉 Chain substitutions:

* First replace words
* Then control occurrence positions

---

## ⚠️ Important Correction (Your Mistake)

❌ You wrote:

```sed
s/\!/./1
s/\!/./2
```

👉 That replaces:

* 1st and **2nd**, NOT 3rd

✔️ Correct:

```sed
s/!/./1
s/!/./3
```

---

## ⚠️ Important Points

* Order matters
* `gi` ensures full + case-insensitive replacement
* Occurrence numbers are per line

---

## 🧠 Memory Trick

👉 **Days → numbers + replace 1st & 3rd `!`**
