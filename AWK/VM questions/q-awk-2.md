## Student Performance Statistics using AWK

---

# Problem Statement

You perform a survey on students to analyze how gender, parental education, and other factors affect performance in math, reading, and writing.

You must compute:

* Mean scores for math, reading, and writing
* Percentage of female and male students
* Percentage of students whose parents attended college
* Conditional probabilities:

  * `P(>79 | college)`
  * `P(>79 | ~college)`

---

# Input Format

```csv
"gender","race_ethnicity","parental_level_of_education","lunch","test_preparation_course","math_score","reading_score","writing_score"
```

---

## Sample Input

```csv
"female","group B","bachelor's degree","standard","none","72","72","74"
"female","group C","some college","standard","completed","69","90","88"
"female","group B","master's degree","standard","none","90","95","93"
"male","group A","associate's degree","free/reduced","none","47","57","44"
"male","group C","some college","standard","none","76","78","75"
"female","group B","associate's degree","standard","none","71","83","78"
"female","group B","some college","standard","completed","88","95","92"
"male","group B","some college","free/reduced","none","40","43","39"
"male","group D","high school","free/reduced","completed","64","64","67"
```

---

## Sample Output

```txt
math average    68.56
read average    75.22
write average   72.22
female percent  55.56
male percent    44.44
parent college% 88.89
P(>79|college)  37.50
P(>79|~college) 0.00
```

---

# Solution

```bash
#!/usr/bin/gawk -f

BEGIN {
    FS = ","
    OFMT = "%.2f"
    OFS = "\t"
}

{
    # remove quotes from numeric fields
    gsub(/"/, "", $6)
    gsub(/"/, "", $7)
    gsub(/"/, "", $8)

    # total scores
    math_sum += $6
    reading_sum += $7
    writing_sum += $8

    # mean of student
    avg_score = ($6 + $7 + $8) / 3

    # check non-college (school)
    if ($3 ~ /school/) {
        noncollege_count++

        if (avg_score >= 80) {
            noncollege_high++
        }
    } 
    else {
        # college students
        if (avg_score >= 80) {
            college_high++
        }
    }
}

# count females
$1 ~ /female/ {
    female_count++
}

END {
    total = NR
    college_count = total - noncollege_count

    print "math average", math_sum / total
    print "read average", reading_sum / total
    print "write average", writing_sum / total

    print "female percent", female_count / total * 100
    print "male percent", (total - female_count) / total * 100

    print "parent college%", college_count / total * 100

    print "P(>79|college)", (college_high / college_count) * 100
    print "P(>79|~college)", (noncollege_high / noncollege_count) * 100
}
```

---

# Explanation

### 🔹 Field Separator

```awk
FS = ","
```

Splits input by comma

---

### 🔹 Removing Quotes

```awk
gsub(/"/, "", $6)
```

Removes quotes from numeric fields

---

### 🔹 Score Calculation

* Adds math, reading, writing scores
* Computes average per student

---

### 🔹 College Detection

```awk
$3 ~ /school/
```

👉 Matches **non-college students** (like "high school")

---

### 🔹 Gender Count

```awk
$1 ~ /female/
```

Counts female students using pattern match

---

### 🔹 Conditional Probability

Uses:

```text
P(A|B) = P(A ∩ B) / P(B)
```

---

### 🔹 Output Formatting

* `OFMT = "%.2f"` → ensures 2 decimal output
* `OFS = "\t"` → separates columns with tabs

---

## 🧠 Key Concepts

* `~` → regex match
* `gsub()` → cleaning data
* `NR` → total rows
* `OFMT` → controls float precision

---

👉 Run:

```bash
./script.awk < input.csv
```
