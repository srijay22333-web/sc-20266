# Employee Hierarchy with Chain using awk

---

## 📝 Problem Statement

Write an **awk script** that:
* Reads a comma-separated file where each line has `employee_id,employee_name,manager_id`
* Stores all employee records (ID, name, and manager ID) into arrays
* For each employee, looks up the **manager's name** using the manager ID
* If an employee has `manager_id = 0`, prints `None` as the manager name (i.e., they are the top-level boss)
* Prints each employee's record as `employee_id,employee_name,manager_name`

---

## 📥 Sample Input

```text
1,Alice,0
2,Bob,1
3,Charlie,1
4,Dave,2
5,Eve,2
```

---

## 📤 Sample Output

```text
1,Alice,None
2,Bob,Alice
3,Charlie,Alice
4,Dave,Bob
5,Eve,Bob
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

        c_name = "None"

        if (mgr[i] != 0) {
            for (j = 1; j <= NR; j++) {
                if (mgr[i] == id[j]) {
                    c_name = name[j]
                    break
                }
            }
        }

        print id[i], name[i], c_name
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

        c_name = "None"

        if (mgr[i] != 0) {
            for (j = 1; j <= NR; j++) {
                if (mgr[i] == id[j]) {
                    c_name = name[j]
                    break
                }
            }
        }

        print id[i], name[i], c_name
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

After Step 2 (Main block reads all 5 lines into arrays):
```
Line 1 → id[1]=1   name[1]="Alice"    mgr[1]=0
Line 2 → id[2]=2   name[2]="Bob"      mgr[2]=1
Line 3 → id[3]=3   name[3]="Charlie"  mgr[3]=1
Line 4 → id[4]=4   name[4]="Dave"     mgr[4]=2
Line 5 → id[5]=5   name[5]="Eve"      mgr[5]=2
```

After Step 3 (END block — outer loop i=1, mgr[1]=0):
```
c_name = "None"  (default reset)
mgr[1] = 0 → mgr[i] != 0 is FALSE → skip inner loop
c_name stays "None"
print → 1,Alice,None ✓
```

After Step 4 (END block — outer loop i=2, mgr[2]=1):
```
c_name = "None"  (default reset)
mgr[2] = 1 → mgr[i] != 0 is TRUE → enter inner loop
  j=1: mgr[2]==id[1]? → 1==1? → YES → c_name = name[1] = "Alice" → break
print → 2,Bob,Alice ✓
```

After Step 5 (END block — outer loop i=3, mgr[3]=1):
```
c_name = "None"  (default reset)
mgr[3] = 1 → mgr[i] != 0 is TRUE → enter inner loop
  j=1: mgr[3]==id[1]? → 1==1? → YES → c_name = name[1] = "Alice" → break
print → 3,Charlie,Alice ✓
```

After Step 6 (END block — outer loop i=4, mgr[4]=2):
```
c_name = "None"  (default reset)
mgr[4] = 2 → mgr[i] != 0 is TRUE → enter inner loop
  j=1: mgr[4]==id[1]? → 2==1? → NO
  j=2: mgr[4]==id[2]? → 2==2? → YES → c_name = name[2] = "Bob" → break
print → 4,Dave,Bob ✓
```

After Step 7 (END block — outer loop i=5, mgr[5]=2):
```
c_name = "None"  (default reset)
mgr[5] = 2 → mgr[i] != 0 is TRUE → enter inner loop
  j=1: mgr[5]==id[1]? → 2==1? → NO
  j=2: mgr[5]==id[2]? → 2==2? → YES → c_name = name[2] = "Bob" → break
print → 5,Eve,Bob ✓
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

Input:  `"2,Bob,1"`
Fields: `$1="2"  $2="Bob"  $3="1"`
Result: Fields correctly split by comma

> 💡 Without `OFS = ","`, the `print id[i], name[i], c_name` would output fields separated by a **space** (AWK's default OFS), giving `2 Bob Alice` instead of `2,Bob,Alice`.

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

Input:  `"3,Charlie,1"` (line 3, so NR=3)
Match:  Every line matches (no pattern)
Result: `id[3]=3  name[3]="Charlie"  mgr[3]=1`

> 💡 We **must** store all records into arrays first because we need to look up manager names by ID. If we tried to process each line as it comes, a manager's record might not have been read yet (e.g., if a subordinate appears before their manager in the file).

---

### 🔹 Default Reset — `c_name = "None"`

```awk
c_name = "None"
```

* `c_name = "None"` → resets the resolved manager name to `"None"` at the **start of every iteration** of the outer `i` loop
* This serves two purposes:
  1. If `mgr[i] == 0` (top-level boss), the inner loop is skipped and `c_name` stays `"None"` — correct!
  2. If `mgr[i] != 0` but no matching `id[j]` is found (invalid manager ID), `c_name` stays `"None"` — safe!

Input:  Employee Alice (i=1), mgr[1]=0
Match:  `mgr[i] != 0` is FALSE → inner loop skipped
Result: `c_name` stays `"None"` → prints `1,Alice,None`

> 💡 This is the **key improvement** over using `if/else`. By defaulting to `"None"` first, we eliminate the `else` branch entirely. The logic becomes: "assume no manager, then try to find one." This is called the **default-then-override** pattern — simpler, safer, and fewer lines of code.

---

### 🔹 Outer If Check — `if (mgr[i] != 0)`

```awk
if (mgr[i] != 0) {
    ...
}
```

* `mgr[i]` → retrieves the manager ID for employee at index `i`
* `!= 0` → checks if this employee **has** a manager (is NOT the top-level boss)
* If `mgr[i]` IS `0`, the entire inner loop is **skipped** — no need for an `else` since `c_name` is already `"None"`

Input:  Employee Bob (i=2), mgr[2]=1
Match:  `mgr[2] != 0` → `1 != 0` → TRUE → enter inner loop
Result: Inner loop will search for manager with id=1

> 💡 Notice this uses `!= 0` (not equal) instead of `== 0` (equal). This flips the logic: instead of "if boss, set None, else search", it becomes "if NOT boss, search" — which is simpler because the default `c_name = "None"` already handles the boss case.

---

### 🔹 Inner For Loop with Break — Manager Name Lookup — `for (j = 1; j <= NR; j++)`

```awk
for (j = 1; j <= NR; j++) {
    if (mgr[i] == id[j]) {
        c_name = name[j]
        break
    }
}
```

* `for (j = 1; j <= NR; j++)` → scans **all** employee records looking for the manager
* `if (mgr[i] == id[j])` → checks: does employee `j`'s ID match the manager ID of employee `i`?
* `c_name = name[j]` → when a match is found, stores the manager's **name** (not ID)
* `break` → **immediately exits** the inner `j` loop — no need to keep searching after a match

Input:  Employee Dave (i=4), mgr[4]=2
Loop:   j=1: id[1]=1, 2==1? NO
        j=2: id[2]=2, 2==2? YES → c_name = name[2] = "Bob" → break (exits loop)
Result: `c_name = "Bob"` → will print `4,Dave,Bob`

> 💡 The `break` statement is crucial for efficiency. Without it, after finding the matching manager at j=2, the loop would **unnecessarily** continue checking j=3, j=4, j=5. Since employee IDs are unique, once we find the match there's no point continuing. For 5 records the difference is tiny, but for thousands of records it saves significant processing time.

---

### 🔹 Print Statement — Final Output — `print id[i], name[i], c_name`

```awk
print id[i], name[i], c_name
```

* `print` → AWK's output statement
* `id[i]` → the current employee's ID
* `name[i]` → the current employee's name
* `c_name` → the resolved manager name (either `"None"` from default or the looked-up name)
* The commas between arguments trigger `OFS` → output separated by `,`

Input:  i=5 → id[5]=5, name[5]="Eve", c_name="Bob"
Match:  Runs for every iteration of outer loop
Result: `"5,Eve,Bob"`

> 💡 The commas in `print id[i], name[i], c_name` are **AWK argument separators**, not literal commas. AWK inserts the `OFS` value (which we set to `,`) between each argument. If you wrote `print id[i] name[i] c_name` (without commas), AWK would **concatenate** them with no separator: `5EveBob`.

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
| 8 | Inner for loop | `for (j = 1; j <= NR; j++)` | Scans all records to find a matching manager ID |
| 9 | Default-then-override | `c_name = "None"` before loop | Sets safe default, overrides only when match found |
| 10 | break statement | `break` | Exits the inner loop immediately after finding the match |
| 11 | If condition (!=) | `if (mgr[i] != 0)` | Skips inner loop for top-level boss |
| 12 | Print with OFS | `print id[i], name[i], c_name` | Outputs multiple fields separated by OFS |

---

## ⚠️ Important Points

* **`c_name` is reset every iteration** — By placing `c_name = "None"` at the top of the outer loop, we guarantee a fresh default for each employee. This prevents stale values from a previous iteration leaking into the current one — the most common bug in nested-loop lookups.

* **`break` exits only the inner loop** — In AWK, `break` exits the **innermost** loop it's inside. So `break` in the `j` loop returns control to the outer `i` loop, which continues to the next employee. It does NOT exit the outer loop.

* **`NR` retains its value in the END block** — After all lines are read, `NR` holds the total count. This is why `for (i = 1; i <= NR; i++)` works in the `END` block. If you mistakenly used `FNR` (which resets per file), the behavior with multiple files would differ.

* **Commas in `print` are argument separators, not literal output** — Writing `print id[i], name[i], c_name` outputs fields separated by `OFS`. Forgetting the commas (`print id[i] name[i] c_name`) concatenates them directly with no separator.

* **`0` as sentinel for no manager** — The solution assumes `manager_id = 0` means "no manager." If the input uses a blank field or `-1` instead, change the condition `if (mgr[i] != 0)` accordingly.

* **Array indexing starts at 1** — We use `NR` as the array index (which starts at 1 for the first line). The loops `for (i = 1; ...)` and `for (j = 1; ...)` must start at 1, not 0, to match.

---

## 🧠 Memory — When to Use Which Operation

| Situation | Use This | Example |
|-----------|----------|---------|
| Need to process all data before producing output | `END { ... }` | Lookups that require the full dataset |
| Split input on a specific delimiter | `FS = ","` | CSV, TSV, colon-separated files |
| Store each field for later use | `array[NR] = $field` | `name[NR] = $2` |
| Iterate over all stored records | `for (i = 1; i <= NR; i++)` | Process each employee in order |
| Search for a matching record | Nested `for (j = 1; j <= NR; j++)` with `if` | Find manager by ID |
| Set a safe default before searching | `variable = "default"` before loop | `c_name = "None"` |
| Stop searching after first match | `break` | Exit inner loop once manager found |
| Handle a special/sentinel value | `if (value != 0)` | Skip search for top-level boss |
| Output fields with a custom separator | `OFS = ","` + `print a, b, c` | Comma-separated output |

> 💡 **Golden Rule — Default-Then-Override Pattern:**
> ```
> Instead of:  if (condition) { set_A } else { set_B }
> Use:         set_default_A; if (!condition) { set_B }
>
> Before:  if (mgr==0) { c_name="None" } else { search... }
> After:   c_name="None"; if (mgr!=0) { search... }
>
> Result:  Simpler logic, no stale values, fewer branches.
> ```
