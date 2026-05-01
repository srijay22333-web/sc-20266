# Filter Departments by Average Salary using awk

---

## 📝 Problem Statement

Write an **awk script** that:
* Reads a CSV file with format `department,salary`
* Skips the first line (header)
* Ignores rows where department is empty
* Ignores rows where salary is not a positive integer
* Counts employees in each department
* Computes average salary per department using integer division
* Computes overall average salary across all valid employees
* Prints only departments where `department_avg ≥ overall_avg`
* Output format: `department,employee_count,average_salary`

---

## 📥 Sample Input

```text
department,salary
Engineering,50000
Engineering,60000
HR,40000
HR,invalid
Sales,45000
Sales,55000
,50000
```

---

## 📤 Sample Output

```text
Engineering,2,55000
Sales,2,50000
```

---

## ✅ Solution — CLI

```bash
awk '
BEGIN { FS = ","; OFS = "," }
NR == 1 { next }
$1 != "" && $2 ~ /^[0-9]+$/ && $2 > 0 {
    dept_count[$1]++
    dept_total[$1] += $2
    overall_total += $2
    overall_count++
}
END {
    overall_avg = int(overall_total / overall_count)
    for (dept in dept_count) {
        dept_avg = int(dept_total[dept] / dept_count[dept])
        if (dept_avg >= overall_avg) {
            print dept, dept_count[dept], dept_avg
        }
    }
}
' input.csv
```

## ✅ Solution — script.awk

```awk
#!/usr/bin/gawk -f

BEGIN {
    FS = ","
    OFS = ","
}

NR == 1 { next }

$1 != "" && $2 ~ /^[0-9]+$/ && $2 > 0 {
    dept_count[$1]++
    dept_total[$1] += $2
    overall_total += $2
    overall_count++
}

END {
    overall_avg = int(overall_total / overall_count)
    for (dept in dept_count) {
        dept_avg = int(dept_total[dept] / dept_count[dept])
        if (dept_avg >= overall_avg) {
            print dept, dept_count[dept], dept_avg
        }
    }
}
```

```bash
awk -f script.awk input.csv
```

---

## 🔍 Explanation

### How it works — step-by-step pipeline trace

After Step 1 (BEGIN block sets delimiters):
```
FS = ","   → fields split on comma
OFS = ","  → output fields joined with comma
No data processed yet.
```

After Step 2 (NR==1 skips header):
```
Line 1: "department,salary" → NR==1 matches → next → SKIPPED
```

After Step 3 (Lines 2–8 validated and accumulated):
```
Line 2: $1="Engineering" $2="50000" → valid ✓
         dept_count["Engineering"]=1  dept_total["Engineering"]=50000
         overall_total=50000  overall_count=1

Line 3: $1="Engineering" $2="60000" → valid ✓
         dept_count["Engineering"]=2  dept_total["Engineering"]=110000
         overall_total=110000  overall_count=2

Line 4: $1="HR" $2="40000" → valid ✓
         dept_count["HR"]=1  dept_total["HR"]=40000
         overall_total=150000  overall_count=3

Line 5: $1="HR" $2="invalid" → $2 ~ /^[0-9]+$/ FAILS → SKIPPED ✗

Line 6: $1="Sales" $2="45000" → valid ✓
         dept_count["Sales"]=1  dept_total["Sales"]=45000
         overall_total=195000  overall_count=4

Line 7: $1="Sales" $2="55000" → valid ✓
         dept_count["Sales"]=2  dept_total["Sales"]=100000
         overall_total=250000  overall_count=5

Line 8: $1="" $2="50000" → $1 != "" FAILS → SKIPPED ✗
```

After Step 4 (END block — compute overall average):
```
overall_avg = int(250000 / 5) = 50000
```

After Step 5 (END block — filter departments):
```
Engineering: dept_avg = int(110000 / 2) = 55000 → 55000 ≥ 50000 → PRINT ✓
HR:          dept_avg = int(40000 / 1)  = 40000 → 40000 < 50000 → SKIP  ✗
Sales:       dept_avg = int(100000 / 2) = 50000 → 50000 ≥ 50000 → PRINT ✓
```

Final output:
```
Engineering,2,55000 ✓
Sales,2,50000 ✓
```

---

### 🔹 Skip Header — `NR == 1 { next }`

```awk
NR == 1 { next }
```

* `NR == 1` → pattern that matches **only** the first line (the header row)
* `next` → skips all remaining rules for this line and jumps to the next input line
* This prevents the header `"department,salary"` from being processed as data

Input:  `"department,salary"`
Match:  NR==1 → TRUE
Result: Line skipped entirely, no arrays updated

> 💡 `next` is different from just skipping with an `if`. It immediately stops processing the current line and moves to the **next input line**. No subsequent rules (like the validation rule) will execute for this line.

---

### 🔹 Validation Pattern — `$1 != "" && $2 ~ /^[0-9]+$/ && $2 > 0`

```awk
$1 != "" && $2 ~ /^[0-9]+$/ && $2 > 0 {
    ...
}
```

* `$1 != ""` → checks that department field is **not empty**
* `$2 ~ /^[0-9]+$/` → checks that salary field contains **only digits** (is a valid number)
  * `~` → regex match operator
  * `^` → anchors to **start** of string
  * `[0-9]+` → one or more digits (character class `[0-9]`, quantifier `+`)
  * `$` → anchors to **end** of string
  * Together: the **entire** field must be only digits — no letters, spaces, or symbols
* `$2 > 0` → ensures the salary is **positive** (excludes `0`)
* `&&` → all three conditions must be TRUE for the block to execute

Input:  `"HR,invalid"` → $1="HR" ✓, $2="invalid" ~ /^[0-9]+$/ → NO ✗ → SKIPPED
Input:  `",50000"` → $1="" → $1 != "" → NO ✗ → SKIPPED
Input:  `"Sales,45000"` → $1="Sales" ✓, $2="45000" ~ /^[0-9]+$/ ✓, $2>0 ✓ → ACCEPTED

> 💡 The regex `^[0-9]+$` is essential. Without the anchors `^` and `$`, a string like `"abc123def"` would match `/[0-9]+/` because it contains digits somewhere. The anchors ensure the **entire** string is digits — nothing before `^` or after `$`.

---

### 🔹 Accumulation Block — Counting and Summing

```awk
{
    dept_count[$1]++
    dept_total[$1] += $2
    overall_total += $2
    overall_count++
}
```

* `dept_count[$1]++` → increments the employee count for this department (associative array keyed by department name)
* `dept_total[$1] += $2` → adds this employee's salary to the department's running total
* `overall_total += $2` → adds salary to the grand total across ALL departments
* `overall_count++` → increments the total valid employee count

Input:  `"Engineering,60000"` (2nd Engineering employee)
Match:  Validation passed
Result: `dept_count["Engineering"]=2  dept_total["Engineering"]=110000  overall_total=110000  overall_count=2`

> 💡 Associative arrays in AWK auto-initialize to `0` for numbers. The first time `dept_count["Engineering"]++` runs, AWK treats the non-existent element as `0` and increments it to `1`. No explicit initialization needed.

---

### 🔹 Overall Average — `int(overall_total / overall_count)`

```awk
overall_avg = int(overall_total / overall_count)
```

* `overall_total / overall_count` → computes the exact floating-point average
* `int(...)` → truncates to integer (floors toward zero) — this is **integer division**
* `overall_avg` → stores the threshold that departments must meet or exceed

Input:  overall_total=250000, overall_count=5
Result: `overall_avg = int(250000 / 5) = int(50000.0) = 50000`

> 💡 `int()` truncates toward zero, not rounds. `int(166.67)` = `166`, not `167`. This matches the problem's "integer division" requirement.

---

### 🔹 Filter and Print — `for (dept in dept_count)` with `if`

```awk
for (dept in dept_count) {
    dept_avg = int(dept_total[dept] / dept_count[dept])
    if (dept_avg >= overall_avg) {
        print dept, dept_count[dept], dept_avg
    }
}
```

* `for (dept in dept_count)` → iterates over **every key** (department name) in the associative array
* `dept_avg = int(dept_total[dept] / dept_count[dept])` → computes this department's integer average
* `if (dept_avg >= overall_avg)` → only prints if department average meets the threshold
* `print dept, dept_count[dept], dept_avg` → outputs department name, count, and average separated by OFS

Input:  dept="HR", dept_total["HR"]=40000, dept_count["HR"]=1, overall_avg=50000
Result: dept_avg=40000, 40000 < 50000 → SKIP (HR not printed)

Input:  dept="Engineering", dept_total=110000, dept_count=2, overall_avg=50000
Result: dept_avg=55000, 55000 ≥ 50000 → PRINT: `"Engineering,2,55000"`

> 💡 `for (dept in dept_count)` does **not** guarantee any specific order. The output order may differ between AWK implementations. The problem says "Do NOT sort output" — meaning any order is acceptable.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---------|--------|--------------|
| 1 | BEGIN block | `BEGIN { ... }` | Runs code before any input is processed |
| 2 | END block | `END { ... }` | Runs code after all input is processed |
| 3 | Field Separator | `FS = ","` | Splits each input line into fields on comma |
| 4 | Output Field Separator | `OFS = ","` | Joins printed fields with comma |
| 5 | Skip header | `NR == 1 { next }` | Skips the first line and moves to line 2 |
| 6 | Regex match | `$2 ~ /^[0-9]+$/` | Checks if field 2 contains only digits |
| 7 | Compound pattern | `cond1 && cond2 && cond3` | All conditions must be true to execute block |
| 8 | Associative arrays | `dept_count[$1]++` | Arrays keyed by string (department name) |
| 9 | Accumulation | `dept_total[$1] += $2` | Adds value to running total per key |
| 10 | Integer division | `int(total / count)` | Truncates division result to integer |
| 11 | For-in loop | `for (dept in dept_count)` | Iterates over all keys in associative array |
| 12 | Conditional filter | `if (dept_avg >= overall_avg)` | Prints only qualifying departments |

---

## ⚠️ Important Points

* **`for (dept in array)` order is NOT guaranteed** — AWK's `for-in` iterates associative array keys in an implementation-dependent order. Do not rely on insertion order. If sorted output is needed, pipe through `sort`.

* **`int()` truncates, it does NOT round** — `int(166.67)` gives `166`, not `167`. This is consistent with "integer division" in most languages. If you need rounding, use `int(value + 0.5)` instead.

* **Regex anchors `^` and `$` are critical** — Without them, `$2 ~ /[0-9]+/` would match `"abc123"` because it contains digits. With `^[0-9]+$`, the **entire** field must be digits only.

* **Associative arrays auto-initialize to 0** — You don't need `dept_count[$1] = 0` before `dept_count[$1]++`. AWK automatically treats a non-existent numeric element as `0`.

* **`next` skips ALL remaining rules** — When `NR == 1 { next }` fires, the validation rule below it is never checked for line 1. This is cleaner than nesting everything inside `NR > 1`.

* **Two-level aggregation pattern** — This problem requires both per-department aggregation AND an overall aggregation. Both must happen during the main block, then the comparison happens in `END`. You cannot compute overall average without processing all lines first.

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|-----------|----------|---------|
| Skip header row | `NR == 1 { next }` | Ignore CSV headers |
| Validate field is numeric | `$2 ~ /^[0-9]+$/` | Reject "invalid", "abc" |
| Check field is not empty | `$1 != ""` | Reject rows with missing department |
| Count per group | `array[key]++` | `dept_count[$1]++` |
| Sum per group | `array[key] += value` | `dept_total[$1] += $2` |
| Integer division | `int(a / b)` | `int(110000 / 2)` = `55000` |
| Iterate all groups | `for (key in array)` | `for (dept in dept_count)` |
| Filter by threshold | `if (value >= threshold)` | Print only above-average depts |
| Combine multiple checks | `cond1 && cond2 && cond3 { ... }` | Validate before accumulating |

> 💡 **Golden Rule — Aggregate-Then-Filter Pattern:**
> ```
> When filtering requires a value computed from ALL data (like overall average),
> you MUST use two phases:
>
> Phase 1 (main block): Accumulate all totals and counts
>   Line "Engineering,50000" → dept_total["Engineering"] += 50000
>                            → overall_total += 50000
>
> Phase 2 (END block):  Compute thresholds, then filter and print
>   overall_avg = int(250000/5) = 50000
>   Engineering avg = 55000 ≥ 50000 → PRINT
>   HR avg = 40000 < 50000 → SKIP
> ```
