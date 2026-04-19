# 📘 Sum Numbers (Corner Case)

---

## 📝 Problem Statement

Given a file containing mixed data (words, symbols, numbers), compute:

👉 The **sum of only valid numeric values**

---

## 🎯 Task

* Iterate through all fields in each line
* Identify **pure numeric values**
* Add them to a running total
* Print the final sum

---

## 📥 Sample Input

```text id="sum_in"
10 apples 20
hello 5 world
30! 40 50
abc 100 xyz
```

---

## 📤 Sample Output

```text id="sum_out"
225
```

---

## ⚠️ Important Note

* Only matches **whole numbers (integers)**
* Values like `30!` or `abc123` are ignored
* Regex used: `^[0-9]+$`

  * Ensures the field contains **only digits**

---

## ✅ Solution — script.awk

```awk id="awk_sum_numbers"
{
    for (i = 1; i <= NF; i++) {
        if ($i ~ /^[0-9]+$/) {
            sum += $i
        }
    }
}

END {
    print sum
}
```

---

## ▶️ Run

```bash id="run_sum"
awk -f script.awk input.txt
```

---

## 🔍 Explanation

* `NF` → number of fields in a line
* Loop checks each field
* `$i ~ /^[0-9]+$/` → matches only integers
* `sum += $i` → adds valid numbers
* `END` block → prints final result

---

## 🧠 Visual Understanding

### ▶ Filtering Phase

```text id="sum_filter"
10      → ✅
apples  → ❌
20      → ✅
30!     → ❌
40      → ✅
```

---

### ▶ Accumulation

```text id="sum_accumulate"
10 + 20 + 5 + 40 + 50 + 100 = 225
```

---

## 🔥 Key Concept

```text id="sum_key"
$i ~ /^[0-9]+$/ → match only pure numeric fields
```

---

## ✅ Alternative (Include Negative Numbers)

```awk id="sum_alt_neg"
{
    for (i = 1; i <= NF; i++) {
        if ($i ~ /^-?[0-9]+$/) {
            sum += $i
        }
    }
}

END {
    print sum
}
```
