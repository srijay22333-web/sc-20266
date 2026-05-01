# Count Direct Subordinates using awk

---

## 📝 Problem Statement

Write an **awk script** that:
* Reads a comma-separated file where each line has `employee_id,employee_name,manager_id`
* Stores all employee records (ID, name, and manager ID) into arrays
* For each employee, counts how many **other employees report directly to them** (i.e., how many employees have `manager_id` equal to this employee's `id`)
* Prints each employee's record as `employee_id,employee_name,direct_subordinates`

---

## 📥 Sample Input

```text
1,Eve,0
2,Hannah,1
3,Eve,0
4,Bob,1
5,Grace,0
6,Eve,1
7,Olivia,5
```

---

## 📤 Sample Output

```text
1,Eve,3
2,Hannah,0
3,Eve,0
4,Bob,0
5,Grace,1
6,Eve,0
7,Olivia,0
```

---

## ✅ Solution — CLI

```bash
awk '
BEGIN { FS = ","; OFS = "," }
{
    id[NR] = $1
    name[NR] = $2
    mgr[NR] = $3
}
END {
    for (i = 1; i <= NR; i++) {

        count = 0

        for (j = 1; j <= NR; j++) {
            if (mgr[j] == id[i]) {
                count++
            }
        }

        print id[i], name[i], count
    }
}
' input.txt
```

## ✅ Solution — script.awk

```awk
#!/usr/bin/gawk -f

BEGIN {
    FS = ","
    OFS = ","
}

{
    id[NR] = $1
    name[NR] = $2
    mgr[NR] = $3
}

END {
    for (i = 1; i <= NR; i++) {

        count = 0

        for (j = 1; j <= NR; j++) {
            if (mgr[j] == id[i]) {
                count++
            }
        }

        print id[i], name[i], count
    }
}
```

```bash
awk -f script.awk input.txt
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

After Step 2 (Main block reads all 7 lines into arrays):
```
Line 1 → id[1]=1   name[1]="Eve"      mgr[1]=0
Line 2 → id[2]=2   name[2]="Hannah"   mgr[2]=1
Line 3 → id[3]=3   name[3]="Eve"      mgr[3]=0
Line 4 → id[4]=4   name[4]="Bob"      mgr[4]=1
Line 5 → id[5]=5   name[5]="Grace"    mgr[5]=0
Line 6 → id[6]=6   name[6]="Eve"      mgr[6]=1
Line 7 → id[7]=7   name[7]="Olivia"   mgr[7]=5
```

After Step 3 (END block — outer loop i=1, id[1]=1, "Who reports to Eve?"):
```
count = 0  (reset)
  j=1: mgr[1]==id[1]? → 0==1? → NO
  j=2: mgr[2]==id[1]? → 1==1? → YES → count=1
  j=3: mgr[3]==id[1]? → 0==1? → NO
  j=4: mgr[4]==id[1]? → 1==1? → YES → count=2
  j=5: mgr[5]==id[1]? → 0==1? → NO
  j=6: mgr[6]==id[1]? → 1==1? → YES → count=3
  j=7: mgr[7]==id[1]? → 5==1? → NO
print → 1,Eve,3 ✓
```

After Step 4 (END block — outer loop i=2, id[2]=2, "Who reports to Hannah?"):
```
count = 0  (reset)
  j=1: mgr[1]==id[2]? → 0==2? → NO
  j=2: mgr[2]==id[2]? → 1==2? → NO
  j=3: mgr[3]==id[2]? → 0==2? → NO
  j=4: mgr[4]==id[2]? → 1==2? → NO
  j=5: mgr[5]==id[2]? → 0==2? → NO
  j=6: mgr[6]==id[2]? → 1==2? → NO
  j=7: mgr[7]==id[2]? → 5==2? → NO
print → 2,Hannah,0 ✓
```

After Step 5 (END block — outer loop i=3, id[3]=3, "Who reports to Eve(3)?"):
```
count = 0  (reset)
  j=1 to j=7: no mgr[j] equals 3
print → 3,Eve,0 ✓
```

After Step 6 (END block — outer loop i=4, id[4]=4, "Who reports to Bob?"):
```
count = 0  (reset)
  j=1 to j=7: no mgr[j] equals 4
print → 4,Bob,0 ✓
```

After Step 7 (END block — outer loop i=5, id[5]=5, "Who reports to Grace?"):
```
count = 0  (reset)
  j=1: mgr[1]==id[5]? → 0==5? → NO
  j=2: mgr[2]==id[5]? → 1==5? → NO
  j=3: mgr[3]==id[5]? → 0==5? → NO
  j=4: mgr[4]==id[5]? → 1==5? → NO
  j=5: mgr[5]==id[5]? → 0==5? → NO
  j=6: mgr[6]==id[5]? → 1==5? → NO
  j=7: mgr[7]==id[5]? → 5==5? → YES → count=1
print → 5,Grace,1 ✓
```

After Step 8 (END block — outer loop i=6, id[6]=6, "Who reports to Eve(6)?"):
```
count = 0  (reset)
  j=1 to j=7: no mgr[j] equals 6
print → 6,Eve,0 ✓
```

After Step 9 (END block — outer loop i=7, id[7]=7, "Who reports to Olivia?"):
```
count = 0  (reset)
  j=1 to j=7: no mgr[j] equals 7
print → 7,Olivia,0 ✓
```

---

### 🔹 BEGIN Block — Setting Delimiters — `BEGIN { FS = ","; OFS = "," }`

```awk
BEGIN {
    FS = ","
    OFS = ","
}
```

* `BEGIN` → executes before any input line is read
* `FS = ","` → sets the **Field Separator** to comma, so `$1`, `$2`, `$3` split on `,`
* `OFS = ","` → sets the **Output Field Separator** to comma, so `print` joins fields with `,`

Input:  `"2,Hannah,1"`
Fields: `$1="2"  $2="Hannah"  $3="1"`
Result: Fields correctly split by comma

> 💡 Without `OFS = ","`, the `print id[i], name[i], count` would output fields separated by a **space** (AWK's default OFS), giving `1 Eve 3` instead of `1,Eve,3`.

---

### 🔹 Main Block — Storing Records into Arrays — `{ id[NR] = $1; name[NR] = $2; mgr[NR] = $3 }`

```awk
{
    id[NR] = $1
    name[NR] = $2
    mgr[NR] = $3
}
```

* `{ ... }` → a pattern-less rule, runs on **every input line**
* `NR` → built-in variable, the **current record (line) number** — auto-increments from 1
* `id[NR] = $1` → stores the employee ID (field 1) into array `id` at index `NR`
* `name[NR] = $2` → stores the employee name (field 2) into array `name` at index `NR`
* `mgr[NR] = $3` → stores the manager ID (field 3) into array `mgr` at index `NR`

Input:  `"7,Olivia,5"` (line 7, so NR=7)
Match:  Every line matches (no pattern)
Result: `id[7]=7  name[7]="Olivia"  mgr[7]=5`

> 💡 We **must** store all records into arrays first because we need to scan **every** employee's `mgr` field to count subordinates. If we tried to count during the main block, employees appearing later in the file would be missed.

---

### 🔹 Default Reset — `count = 0`

```awk
count = 0
```

* `count = 0` → resets the subordinate counter to `0` at the **start of every iteration** of the outer `i` loop
* This ensures each employee starts with a fresh count — no leftover value from the previous employee

Input:  i=2 (Hannah), previous iteration had count=3 (Eve)
Match:  Runs at the start of every outer loop iteration
Result: `count` resets to `0`, preventing Eve's count from leaking into Hannah's result

> 💡 This is the **same default-reset pattern** from the previous problem (`c_name = "None"`). Here we reset a number instead of a string. The principle is identical: always reset before the inner loop to prevent stale values.

---

### 🔹 Inner For Loop — Counting Subordinates — `for (j = 1; j <= NR; j++)`

```awk
for (j = 1; j <= NR; j++) {
    if (mgr[j] == id[i]) {
        count++
    }
}
```

* `for (j = 1; j <= NR; j++)` → scans **all** employee records
* `if (mgr[j] == id[i])` → checks: does employee `j` report to employee `i`? (i.e., is `j`'s manager ID the same as `i`'s employee ID?)
* `count++` → increments the counter by 1 for each match found

Input:  i=1 (Eve, id=1)
Loop:   j=2: mgr[2]=1, 1==1? YES → count=1
        j=4: mgr[4]=1, 1==1? YES → count=2
        j=6: mgr[6]=1, 1==1? YES → count=3
Result: `count = 3` → three employees report to Eve

> 💡 **No `break` here!** Unlike the previous problem (find ONE manager name), this problem requires counting ALL matches. Using `break` would stop after the first subordinate, giving `count=1` instead of `count=3` for Eve. **Rule: use `break` when finding ONE match, skip `break` when counting ALL matches.**

---

### 🔹 Print Statement — Final Output — `print id[i], name[i], count`

```awk
print id[i], name[i], count
```

* `print` → AWK's output statement
* `id[i]` → the current employee's ID
* `name[i]` → the current employee's name
* `count` → the number of direct subordinates found
* The commas between arguments trigger `OFS` → output separated by `,`

Input:  i=5 → id[5]=5, name[5]="Grace", count=1
Match:  Runs for every iteration of outer loop
Result: `"5,Grace,1"`

> 💡 The commas in `print id[i], name[i], count` are **AWK argument separators**, not literal commas. AWK inserts the `OFS` value (which we set to `,`) between each argument.

---

## 🧩 Concepts Used in This Solution

| # | Concept | Syntax | What It Does |
|---|---------|--------|--------------|
| 1 | BEGIN block | `BEGIN { ... }` | Runs code before any input is processed |
| 2 | END block | `END { ... }` | Runs code after all input is processed |
| 3 | Field Separator | `FS = ","` | Splits each input line into fields on comma |
| 4 | Output Field Separator | `OFS = ","` | Joins printed fields with comma |
| 5 | Arrays | `id[NR] = $1` | Stores values in associative arrays indexed by line number |
| 6 | NR variable | `NR` | Built-in variable holding the current/total record number |
| 7 | Outer for loop | `for (i = 1; i <= NR; i++)` | Iterates over each employee in order |
| 8 | Inner for loop | `for (j = 1; j <= NR; j++)` | Scans all records to count subordinates |
| 9 | Default reset | `count = 0` | Resets counter before each inner loop scan |
| 10 | Increment operator | `count++` | Adds 1 to count for each matching subordinate |
| 11 | Equality comparison | `mgr[j] == id[i]` | Checks if employee j reports to employee i |
| 12 | Print with OFS | `print id[i], name[i], count` | Outputs multiple fields separated by OFS |

---

## ⚠️ Important Points

* **No `break` in this solution** — Unlike the manager-name-lookup problem, this problem counts **all** subordinates. Using `break` would stop after the first match and give wrong counts. **Rule: `break` = find ONE, no `break` = count ALL.**

* **`count` must be reset every iteration** — If you forget `count = 0` at the top of the outer loop, counts will **accumulate** across employees. Eve (3 subordinates) + Hannah (0) would still show Hannah with count=3.

* **The comparison is `mgr[j] == id[i]`, not `mgr[i] == id[j]`** — Notice the direction is reversed compared to the manager-name problem:
  * Manager name lookup: `mgr[i] == id[j]` → "find j whose id matches i's manager"
  * Subordinate count: `mgr[j] == id[i]` → "find all j whose manager is i"
  * Mixing these up is the #1 bug in this type of problem.

* **Employees with `manager_id = 0` are NOT special here** — In the manager-name problem, we skipped the inner loop for boss employees (`mgr[i] == 0`). Here, every employee needs the inner loop to count subordinates — even a boss (like Eve with id=1) can have subordinates, and an employee with a manager (like Grace with id=5) can also have subordinates.

* **Duplicate names don't matter** — The input has three employees named "Eve" (ids 1, 3, 6). This is fine because we identify employees by their array index and `id`, not by name. Each gets their own correct count.

* **`NR` retains its value in the END block** — After all 7 lines are read, `NR` holds 7. This is why `for (i = 1; i <= NR; i++)` correctly iterates over all employees in the `END` block.

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|-----------|----------|---------|
| Find ONE matching record by ID | Inner `j` loop + `break` | Manager name lookup |
| Count ALL matching records | Inner `j` loop + `count++` (no break) | Subordinate count |
| Set safe default before searching | `variable = default` before loop | `count = 0` or `c_name = "None"` |
| Check "does j belong to i?" | `mgr[j] == id[i]` | Count who reports to i |
| Check "who does i belong to?" | `mgr[i] == id[j]` | Find i's manager |
| Store all records for later processing | `array[NR] = $field` in main block | `id[NR] = $1` |
| Process after all data is loaded | `END { ... }` | Lookups and counts |
| Output fields with custom separator | `OFS = ","` + `print a, b, c` | Comma-separated output |

> 💡 **Golden Rule — Break vs No Break:**
> ```
> FIND ONE match   → use break    → stops at first match
>   Example: Find manager name → mgr[i]==id[j] → c_name=name[j] → break
>
> COUNT ALL matches → no break    → scans every record
>   Example: Count subordinates → mgr[j]==id[i] → count++ → keep going
>
> Ask yourself: "Do I need ONE result or ALL results?"
>   ONE  → break
>   ALL  → no break
> ```
