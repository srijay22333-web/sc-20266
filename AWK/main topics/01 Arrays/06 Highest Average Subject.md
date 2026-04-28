# 📘 Highest Average Subject per Student (AWK)

---

## 📝 Problem Statement

You are given a CSV input where each line contains:

```text
id,name,subject,marks
```

---

### 📌 Field Meaning

* `id` → record ID
* `name` → student name
* `subject` → subject name
* `marks` → marks obtained

---

## 🎯 Your Task

* For each `(name, subject)`:

  * Compute the **average marks**
* For each `name`:

  * Find the subject with the **highest average**
* Print:

```text
name subject
```

---

## 📥 Sample Input

```text
1,Alice,math,10
2,Alice,math,20
3,Alice,science,30
4,Bob,math,40
5,Bob,science,20
6,Bob,science,40
```

---

## 📤 Sample Output

```text
Alice science
Bob math
```

---

# 💻 Solution (script.awk)

```awk
#!/usr/bin/gawk -f

BEGIN {
	FS = ","
}

{
	name = $2
	subject = $3
	value = $4

	group[name,subject] += value
	count[name,subject]++
}

END {
	for (k in group) {

		split(k, a, SUBSEP)

		name = a[1]
		subject = a[2]

		avg = group[k] / count[k]

		if (!(name in max) || avg > max[name]) {
			max[name] = avg
			best[name] = subject
		}
	}

	n = asorti(best, s, "@ind_str_asc")

	for (i = 1; i <= n; i++) {
		store = s[i]
		print store, best[store]
	}
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 Step 1: Group Data

```awk
group[name,subject] += value
count[name,subject]++
```

👉 Stores total marks and count

---

## 🔹 Step 2: Compute Average

```awk
avg = group[k] / count[k]
```

---

## 🔹 Step 3: Track Maximum

```awk
if (!(name in max) || avg > max[name])
```

👉 Finds best subject per student

---

## 🔹 Step 4: Sort Output

```awk
asorti(best, s, "@ind_str_asc")
```

👉 Sort names alphabetically

---

# 🎯 Key Idea

👉
**group → average → compare → select best**

---

# ⚠️ Important Points

* Use `SUBSEP` for multi-key arrays
* Compute average in `END` block
* Output is sorted by name

---

# 🧠 Memory Trick

👉
**sum → count → avg → max**

---

# 🚀 Final Insight

This is a **combined AWK pattern**:

```text
multi-key grouping + aggregation + selection + sorting
```

---

# 💪 Practice Check

If Alice has:

```text
Alice math 50
Alice science 50
```

👉 Which subject prints?

---

Answer: **last encountered wins (tie case)**

---

You’re now solving **advanced AWK aggregation problems cleanly** 🚀
