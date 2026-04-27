# 📘 Find Minimum Value per Name (AWK)

---

## 📝 Problem Statement

You are given input where each line contains:

```text
Name Value
```

---

## 🎯 Your Task

* For each name:

  * Find the **minimum value**
* Print:

```text
Name MinValue
```

---

## 📥 Sample Input

```text
Alice 10
Alice 25
Bob 20
Alice 15
Bob 30
```

---

## 📤 Sample Output

```text
Alice 10
Bob 20
```

---

# 💻 Solution (script.awk)

```awk
#!/usr/bin/gawk -f

BEGIN {
	FS = " "
}

{
	name = $1
	value = $2

	if (!(name in min) || value < min[name]) {
		min[name] = value
	}
}

END {
	n = asorti(min, s, "@ind_str_asc")

	for (i = 1; i <= n; i++) {
		print s[i], min[s[i]]
	}
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 Track Minimum

```awk
if (!(name in min) || value < min[name])
```

👉 First time OR smaller value → update

---

## 🔹 Store Minimum

```awk
min[name] = value
```

---

## 🔹 Sort Names

```awk
asorti(min, s, "@ind_str_asc")
```

👉 Sorts keys (names) alphabetically

---

## 🔹 Print in Order

```awk
print s[i], min[s[i]]
```

👉 Access sorted keys

---

# 🎯 Key Idea

👉
**first OR smaller → update → sort → print**

---

# ⚠️ Important Points

* `asorti()` sorts **keys**, not values
* `"@ind_str_asc"` → alphabetical order
* Works even if input order is random

---

# 🧠 Memory Trick

👉
**min = <
asorti = sort keys**

---
