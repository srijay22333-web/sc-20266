# 🧠 AWK Arrays — The Complete Exam Mastery Guide

> _"Every AWK exam problem is just: pick a key, pick an operation, print at the END."_

---

## 1. What is an Array in AWK?

### Simple Analogy — A Scoreboard

Imagine a **cricket scoreboard** at a school match:

```
Player Name  →  Runs Scored
───────────     ───────────
Amit         →  45
Priya        →  32
Zara         →  58
```

That's it. That's an AWK array.

- **Key** = the label (player name)
- **Value** = the stored data (runs scored)
- **Array** = the entire scoreboard

### In AWK Terms

```
scoreboard["Amit"]  = 45
scoreboard["Priya"] = 32
scoreboard["Zara"]  = 58
```

> 🧠 **Key insight:** AWK arrays are always `key → value`. There is no order. No index 0, 1, 2. Just labels pointing to data.

### What Makes AWK Arrays Special

| Feature | AWK Arrays | Normal Arrays (C/Java) |
|---------|-----------|----------------------|
| Index type | **Any string** | Numbers only (0,1,2…) |
| Declaration | **Not needed** | Must declare size |
| Default value | **0 or ""** | Undefined/garbage |
| Order | **No order** | Sequential |

> 💡 AWK arrays are really **dictionaries** — like Python `dict` or Java `HashMap`.

---

## 2. Core Syntax — Just 3 Lines to Know

### 📌 Store a value

```awk
arr[key] = value
```

```
arr["Amit"] = 45
# Scoreboard: Amit → 45
```

### 📌 Count occurrences

```awk
arr[key]++
```

```
arr["Amit"]++
# First time:  Amit → 0+1 = 1
# Second time: Amit → 1+1 = 2
```

> ⚡ `arr[key]++` means: "I saw this key one more time."

### 📌 Accumulate / Sum

```awk
arr[key] += value
```

```
arr["Amit"] += 10   # Amit → 0+10 = 10
arr["Amit"] += 5    # Amit → 10+5 = 15
```

> ⚡ `arr[key] += value` means: "Add this much more to this key."

### What Happens Internally?

| Operation | If key exists | If key is NEW |
|-----------|--------------|---------------|
| `arr[k] = v` | Overwrites old value | Creates entry with value `v` |
| `arr[k]++` | Increments by 1 | Auto-creates with `0`, then adds `1` → result is `1` |
| `arr[k] += v` | Adds `v` to existing | Auto-creates with `0`, then adds `v` → result is `v` |

> 🧠 **AWK never crashes on missing keys.** It silently creates them with value `0` (numeric) or `""` (string).

---

## 3. Visual Example — Watch the Array Build

### Input (`data.txt`)

```
Alice 10
Bob 20
Alice 5
Bob 3
Alice 7
```

### Code: `{ arr[$1] += $2 }`

### Step-by-Step Array State

| Step | Line Read | `$1` | `$2` | Operation | Array State |
|------|-----------|------|------|-----------|-------------|
| 1 | `Alice 10` | Alice | 10 | `arr["Alice"] = 0 + 10` | `Alice→10` |
| 2 | `Bob 20` | Bob | 20 | `arr["Bob"] = 0 + 20` | `Alice→10, Bob→20` |
| 3 | `Alice 5` | Alice | 5 | `arr["Alice"] = 10 + 5` | `Alice→15, Bob→20` |
| 4 | `Bob 3` | Bob | 3 | `arr["Bob"] = 20 + 3` | `Alice→15, Bob→23` |
| 5 | `Alice 7` | Alice | 7 | `arr["Alice"] = 15 + 7` | `Alice→22, Bob→23` |

### Final Array

| Key | Value |
|-----|-------|
| Alice | 22 |
| Bob | 23 |

### Code: Printing

```awk
{ arr[$1] += $2 }
END {
    for (name in arr)
        print name, arr[name]
}
```

**Output:**
```
Alice 22
Bob 23
```

> ⚠️ **`for (key in arr)`** — order is NOT guaranteed. Don't assume alphabetical!

---

## 4. Core Patterns — The Only 4 You Need for Exams

---

### ✅ Pattern 1: COUNT

> _"How many times does each X appear?"_

```awk
{ count[$1]++ }
END { for (k in count) print k, count[k] }
```

#### Visual — Input:

```
apple
banana
apple
apple
banana
```

| Step | Key | count[key] |
|------|-----|-----------|
| 1 | apple | 1 |
| 2 | banana | 1 |
| 3 | apple | 2 |
| 4 | apple | 3 |
| 5 | banana | 2 |

**Output:** `apple 3`, `banana 2`

---

### ✅ Pattern 2: SUM

> _"What is the total per X?"_

```awk
{ sum[$1] += $2 }
END { for (k in sum) print k, sum[k] }
```

#### Visual — Input:

```
Sales 500
Marketing 300
Sales 200
Marketing 100
```

| Step | Key | sum[key] |
|------|-----|---------|
| 1 | Sales | 500 |
| 2 | Marketing | 300 |
| 3 | Sales | 700 |
| 4 | Marketing | 400 |

**Output:** `Sales 700`, `Marketing 400`

---

### ✅ Pattern 3: MAX

> _"What is the highest value per X?"_

```awk
{
    if ($2 > max[$1])
        max[$1] = $2
}
END { for (k in max) print k, max[k] }
```

#### Visual — Input:

```
Amit 85
Priya 92
Amit 90
Priya 88
Amit 78
```

| Step | Key | Condition | max[key] |
|------|-----|-----------|---------|
| 1 | Amit | 85 > 0 ✅ | 85 |
| 2 | Priya | 92 > 0 ✅ | 92 |
| 3 | Amit | 90 > 85 ✅ | 90 |
| 4 | Priya | 88 > 92 ❌ | 92 |
| 5 | Amit | 78 > 90 ❌ | 90 |

**Output:** `Amit 90`, `Priya 92`

> 💡 Works because AWK default is `0`. If values can be negative, use the MIN pattern below.

---

### ✅ Pattern 4: MIN

> _"What is the lowest value per X?"_

```awk
{
    if (!($1 in min) || $2 < min[$1])
        min[$1] = $2
}
END { for (k in min) print k, min[k] }
```

#### Why the extra check `!($1 in min)`?

| Problem | `if ($2 < min[$1])` alone | With `!($1 in min)` check |
|---------|--------------------------|--------------------------|
| First time we see "Amit" | `85 < 0`? **No!** Skipped! ❌ | Key doesn't exist yet → **force store** ✅ |

> ⚠️ **MAX works with default 0 (any positive number > 0). MIN does NOT work with default 0 (most numbers are NOT < 0).** You MUST use the `in` check for MIN.

#### Visual — Input:

```
Amit 85
Priya 92
Amit 90
Priya 70
Amit 78
```

| Step | Key | Condition | min[key] |
|------|-----|-----------|---------|
| 1 | Amit | Not in min ✅ | 85 |
| 2 | Priya | Not in min ✅ | 92 |
| 3 | Amit | 90 < 85 ❌ | 85 |
| 4 | Priya | 70 < 92 ✅ | 70 |
| 5 | Amit | 78 < 85 ✅ | 78 |

**Output:** `Amit 78`, `Priya 70`

---

## 5. Pattern Recognition Guide — Read the Question, See the Pattern

| The Question Says… | You Think… | Pattern |
|--------------------|-----------|---------|
| "count occurrences" | How many times? | `arr[k]++` |
| "how many times" | Count! | `arr[k]++` |
| "frequency" | Count! | `arr[k]++` |
| "total per" / "sum of" | Add them up | `arr[k] += $N` |
| "total salary by dept" | Sum! | `arr[k] += $N` |
| "highest" / "maximum" | Keep the biggest | `if ($N > max[k])` |
| "top scorer" | Max! | `if ($N > max[k])` |
| "lowest" / "minimum" | Keep the smallest (with `in` check) | `if (!(k in min) \|\| $N < min[k])` |
| "cheapest" | Min! | `if (!(k in min) \|\| $N < min[k])` |
| "average per" | Sum + Count, divide at END | `sum[k]+=$N; count[k]++` |
| "group and find best" | Combine patterns | Sum + Max or Count + Max |

### ⚡ Speed Rule

```
Read the question → Find the VERB → Match the PATTERN

"count"   → arr[k]++
"total"   → arr[k] += value
"highest" → if (value > max[k])
"lowest"  → if (!(k in min) || value < min[k])
"average" → sum + count → divide in END
```

---

## 6. Problem Breakdown Framework — 5 Questions for ANY Problem

Before writing a single line of code, answer these:

```
┌─────────────────────────────────────────────────┐
│          THE 5-QUESTION FRAMEWORK               │
├─────────────────────────────────────────────────┤
│ Q1. What is ONE LINE of input?                  │
│     → Identify fields: $1, $2, $3…              │
│                                                 │
│ Q2. What is the KEY? (Group by what?)            │
│     → Usually $1 (name, dept, category)          │
│                                                 │
│ Q3. What is the VALUE? (What do I track?)        │
│     → Usually $2 or $3 (score, salary, count)    │
│                                                 │
│ Q4. What OPERATION? (Count/Sum/Max/Min?)         │
│     → Read the verb in the question              │
│                                                 │
│ Q5. WHEN do I print? (Every line or at END?)     │
│     → Almost always: END block                   │
└─────────────────────────────────────────────────┘
```

### Example Walkthrough

> **Question:** "Print the total salary per department."

| # | Question | Answer |
|---|----------|--------|
| Q1 | One line? | `Engineering 50000` → `$1`=dept, `$2`=salary |
| Q2 | Key? | `$1` (department) |
| Q3 | Value? | `$2` (salary) |
| Q4 | Operation? | "total" → **SUM** |
| Q5 | When print? | After all lines → **END** |

**Code (writes itself):**

```awk
{ sum[$1] += $2 }
END { for (dept in sum) print dept, sum[dept] }
```

---

## 7. Real Exam Problems — Full Solutions

---

### 📝 Problem 1: Count (Easy)

> **Count how many times each fruit appears.**

**Input:**
```
apple
banana
apple
cherry
banana
apple
```

**Framework:**

| # | Answer |
|---|--------|
| Q1 | One word per line: `$1` = fruit |
| Q2 | Key = `$1` (fruit name) |
| Q3 | Value = count (no field needed) |
| Q4 | "count" → `arr[k]++` |
| Q5 | END |

**Code:**
```awk
{ count[$1]++ }
END { for (f in count) print f, count[f] }
```

**Array Trace:**

| Line | Operation | Array State |
|------|-----------|-------------|
| apple | count["apple"]=1 | apple→1 |
| banana | count["banana"]=1 | apple→1, banana→1 |
| apple | count["apple"]=2 | apple→2, banana→1 |
| cherry | count["cherry"]=1 | apple→2, banana→1, cherry→1 |
| banana | count["banana"]=2 | apple→2, banana→2, cherry→1 |
| apple | count["apple"]=3 | apple→3, banana→2, cherry→1 |

**Output:**
```
apple 3
banana 2
cherry 1
```

---

### 📝 Problem 2: Sum (Easy)

> **Find total marks per student.**

**Input (`marks.txt`):**
```
Amit 85
Priya 90
Amit 78
Priya 88
Amit 92
```

**Code:**
```awk
{ total[$1] += $2 }
END { for (name in total) print name, total[name] }
```

**Array Trace:**

| Line | Operation | total[] |
|------|-----------|---------|
| Amit 85 | total["Amit"] = 0+85 | Amit→85 |
| Priya 90 | total["Priya"] = 0+90 | Amit→85, Priya→90 |
| Amit 78 | total["Amit"] = 85+78 | Amit→163, Priya→90 |
| Priya 88 | total["Priya"] = 90+88 | Amit→163, Priya→178 |
| Amit 92 | total["Amit"] = 163+92 | Amit→255, Priya→178 |

**Output:**
```
Amit 255
Priya 178
```

---

### 📝 Problem 3: Max (Medium)

> **Find the highest score per student.**

**Input:**
```
Amit 85
Priya 92
Amit 90
Priya 88
Zara 95
Amit 78
Zara 91
```

**Code:**
```awk
{ if ($2 > max[$1]) max[$1] = $2 }
END { for (name in max) print name, max[name] }
```

**Array Trace:**

| Line | Condition | max[] |
|------|-----------|-------|
| Amit 85 | 85 > 0 ✅ | Amit→85 |
| Priya 92 | 92 > 0 ✅ | Amit→85, Priya→92 |
| Amit 90 | 90 > 85 ✅ | Amit→90, Priya→92 |
| Priya 88 | 88 > 92 ❌ | Amit→90, Priya→92 |
| Zara 95 | 95 > 0 ✅ | Amit→90, Priya→92, Zara→95 |
| Amit 78 | 78 > 90 ❌ | Amit→90, Priya→92, Zara→95 |
| Zara 91 | 91 > 95 ❌ | Amit→90, Priya→92, Zara→95 |

**Output:**
```
Amit 90
Priya 92
Zara 95
```

---

### 📝 Problem 4: Group + Max (Exam Level)

> **Find the top scorer in each department.**

**Input (`employees.txt`):**
```
Engineering Amit 85000
Marketing Priya 72000
Engineering Zara 92000
Marketing Ravi 68000
Engineering Kiran 88000
HR Sana 75000
Marketing Neha 78000
HR Faiz 71000
```

**Framework:**

| # | Answer |
|---|--------|
| Q1 | `$1`=dept, `$2`=name, `$3`=salary |
| Q2 | Key = `$1` (department) |
| Q3 | Value = `$3` (salary) — but also need `$2` (name) |
| Q4 | "top" → **MAX** + store who |
| Q5 | END |

**Code:**
```awk
{
    if ($3 > max[$1]) {
        max[$1] = $3
        who[$1] = $2
    }
}
END {
    for (dept in max)
        print dept, who[dept], max[dept]
}
```

> 🧠 **Key trick:** Use TWO arrays — `max[]` for the salary, `who[]` for the person's name. Same key (department) in both.

**Array Trace:**

| Line | Condition | max[] | who[] |
|------|-----------|-------|-------|
| Eng Amit 85000 | 85000 > 0 ✅ | Eng→85000 | Eng→Amit |
| Mkt Priya 72000 | 72000 > 0 ✅ | Eng→85000, Mkt→72000 | Eng→Amit, Mkt→Priya |
| Eng Zara 92000 | 92000 > 85000 ✅ | Eng→92000, Mkt→72000 | Eng→Zara, Mkt→Priya |
| Mkt Ravi 68000 | 68000 > 72000 ❌ | _(no change)_ | _(no change)_ |
| Eng Kiran 88000 | 88000 > 92000 ❌ | _(no change)_ | _(no change)_ |
| HR Sana 75000 | 75000 > 0 ✅ | …, HR→75000 | …, HR→Sana |
| Mkt Neha 78000 | 78000 > 72000 ✅ | Mkt→78000 | Mkt→Neha |
| HR Faiz 71000 | 71000 > 75000 ❌ | _(no change)_ | _(no change)_ |

**Output:**
```
Engineering Zara 92000
Marketing Neha 78000
HR Sana 75000
```

---

### 📝 Problem 5: Average (Exam Level)

> **Find the average marks per subject.**

**Input:**
```
Math 80
Science 90
Math 70
Science 85
Math 95
English 88
```

**Code:**
```awk
{
    sum[$1] += $2
    count[$1]++
}
END {
    for (subj in sum)
        printf "%s %.2f\n", subj, sum[subj] / count[subj]
}
```

**Array Trace:**

| Line | sum[] | count[] |
|------|-------|---------|
| Math 80 | Math→80 | Math→1 |
| Science 90 | Sci→90 | Sci→1 |
| Math 70 | Math→150 | Math→2 |
| Science 85 | Sci→175 | Sci→2 |
| Math 95 | Math→245 | Math→3 |
| English 88 | Eng→88 | Eng→1 |

**Output:**
```
Math 81.67
Science 87.50
English 88.00
```

> 🧠 **Average = SUM + COUNT combined.** Divide at the END, never during processing.

---

## 8. Common Mistakes

### ❌ Mistake 1: Wrong Key Selection

```awk
# Question: "Total salary per department"
# Input: Engineering Amit 85000

{ sum[$2] += $3 }   # ❌ WRONG — $2 is name, not department!
{ sum[$1] += $3 }   # ✅ CORRECT — $1 is department
```

> 🔑 Always re-read the input format. `$1` is not always the key.

---

### ❌ Mistake 2: Forgetting the END Block

```awk
# ❌ WRONG — prints for EVERY line!
{ count[$1]++; for (k in count) print k, count[k] }

# ✅ CORRECT — prints ONCE at the end
{ count[$1]++ }
END { for (k in count) print k, count[k] }
```

> 📌 Rule: **Processing in `{ }`, Printing in `END { }`**

---

### ❌ Mistake 3: Using = Instead of +=

```awk
{ sum[$1] = $2 }    # ❌ OVERWRITES — only keeps LAST value!
{ sum[$1] += $2 }   # ✅ ACCUMULATES — adds to previous total
```

| Input | `= $2` (wrong) | `+= $2` (correct) |
|-------|----------------|-------------------|
| Amit 85 | Amit→85 | Amit→85 |
| Amit 90 | Amit→90 ❌ (85 lost!) | Amit→175 ✅ |

---

### ❌ Mistake 4: MIN Without Existence Check

```awk
# ❌ WRONG — default 0 is less than any positive number!
{ if ($2 < min[$1]) min[$1] = $2 }

# ✅ CORRECT
{ if (!($1 in min) || $2 < min[$1]) min[$1] = $2 }
```

---

### ❌ Mistake 5: Printing key Instead of arr[key]

```awk
# ❌ WRONG — prints the key twice
END { for (k in arr) print k, k }

# ✅ CORRECT — prints key and its value
END { for (k in arr) print k, arr[k] }
```

---

## 9. Memory Tricks — One-Liners to Remember

| # | Trick |
|---|-------|
| 1 | **COUNT** = `++` (tally marks: one more!) |
| 2 | **SUM** = `+=` (add to the pile) |
| 3 | **MAX** = `if bigger, replace` (king of the hill) |
| 4 | **MIN** = `if exists AND smaller, replace` (limbo — check the bar first) |
| 5 | **AVG** = `sum + count`, divide at END (never mid-game) |
| 6 | **Key** = what you group BY. **Value** = what you track. |
| 7 | **`{ }` = kitchen** (cook data). **`END { }` = dining table** (serve results). |

### The Master Formula

```
{ operation[KEY] ○= VALUE }
END { for (k in operation) print k, operation[k] }

Where ○ is:  nothing (count: ++)
             +       (sum: +=)
             custom  (max/min: if-then)
```

---

## 10. Practice Section

---

### 🟢 Easy (3 Problems)

**E1.** Count how many times each word appears in a file.
```
hello
world
hello
hello
world
```
_Expected: hello 3, world 2_

<details>
<summary>✅ Solution</summary>

```awk
{ count[$1]++ }
END { for (w in count) print w, count[w] }
```
</details>

---

**E2.** Find total sales per city.
```
Mumbai 5000
Delhi 3000
Mumbai 2000
Delhi 4000
```
_Expected: Mumbai 7000, Delhi 7000_

<details>
<summary>✅ Solution</summary>

```awk
{ sales[$1] += $2 }
END { for (city in sales) print city, sales[city] }
```
</details>

---

**E3.** Count how many students are in each class.
```
10A Amit
10B Priya
10A Zara
10A Ravi
10B Kiran
```
_Expected: 10A 3, 10B 2_

<details>
<summary>✅ Solution</summary>

```awk
{ count[$1]++ }
END { for (cls in count) print cls, count[cls] }
```
</details>

---

### 🟡 Medium (3 Problems)

**M1.** Find the highest temperature per city.
```
Mumbai 35
Delhi 42
Mumbai 38
Delhi 40
Mumbai 33
Delhi 45
```
_Expected: Mumbai 38, Delhi 45_

<details>
<summary>✅ Solution</summary>

```awk
{ if ($2 > max[$1]) max[$1] = $2 }
END { for (city in max) print city, max[city] }
```
</details>

---

**M2.** Find the average marks per subject.
```
Math 80
Science 90
Math 60
Science 70
Math 100
Science 80
```
_Expected: Math 80.00, Science 80.00_

<details>
<summary>✅ Solution</summary>

```awk
{ sum[$1] += $2; count[$1]++ }
END { for (s in sum) printf "%s %.2f\n", s, sum[s]/count[s] }
```
</details>

---

**M3.** Find the minimum price per product.
```
Laptop 45000
Phone 25000
Laptop 42000
Phone 28000
Laptop 48000
Phone 22000
```
_Expected: Laptop 42000, Phone 22000_

<details>
<summary>✅ Solution</summary>

```awk
{ if (!($1 in min) || $2 < min[$1]) min[$1] = $2 }
END { for (p in min) print p, min[p] }
```
</details>

---

### 🔴 Exam Level (2 Problems)

**X1.** Find the top scorer (name and marks) per department.
```
CS Amit 92
EC Priya 88
CS Zara 95
EC Ravi 91
CS Kiran 89
EC Neha 93
```
_Expected: CS Zara 95, EC Neha 93_

<details>
<summary>✅ Solution</summary>

**Framework:**
| # | Answer |
|---|--------|
| Q1 | `$1`=dept, `$2`=name, `$3`=marks |
| Q2 | Key = `$1` |
| Q3 | Track `$3` (marks) AND `$2` (name) |
| Q4 | MAX (two arrays) |
| Q5 | END |

```awk
{
    if ($3 > max[$1]) {
        max[$1] = $3
        who[$1] = $2
    }
}
END { for (d in max) print d, who[d], max[d] }
```
</details>

---

**X2.** Print students who scored above the class average.

```
10A Amit 85
10A Priya 92
10A Zara 78
10B Ravi 90
10B Kiran 70
10B Neha 88
```

_Hint: This needs **two passes** — first calculate averages, then compare._

<details>
<summary>✅ Solution</summary>

**Approach:** Store all data, compute averages in END, then compare.

```awk
{
    sum[$1] += $3
    count[$1]++
    # Store each line's data
    class[NR] = $1
    name[NR]  = $2
    marks[NR] = $3
}
END {
    # Calculate averages
    for (c in sum)
        avg[c] = sum[c] / count[c]

    # Print above average
    for (i = 1; i <= NR; i++) {
        if (marks[i] > avg[class[i]])
            print class[i], name[i], marks[i], "(avg:", avg[class[i]] ")"
    }
}
```

**Output:**
```
10A Amit 85 (avg: 85)       ← Not printed (not > avg)
10A Priya 92 (avg: 85)      ← Printed ✅
10B Ravi 90 (avg: 82.6667)  ← Printed ✅
10B Neha 88 (avg: 82.6667)  ← Printed ✅
```

> 🧠 **Key insight:** When you need ALL data + computed results, store everything in indexed arrays (`NR` as key), compute in `END`, then loop again.
</details>

---

## ⚡ Exam Shortcut Summary

```
┌──────────────────────────────────────────────────────┐
│              AWK ARRAY CHEAT SHEET                   │
├──────────────────────────────────────────────────────┤
│                                                      │
│  COUNT:  { arr[$1]++ }                               │
│  SUM:    { arr[$1] += $2 }                           │
│  MAX:    { if ($2 > max[$1]) max[$1] = $2 }          │
│  MIN:    { if (!($1 in min) || $2 < min[$1])         │
│               min[$1] = $2 }                         │
│  AVG:    { s[$1]+=$2; c[$1]++ }                      │
│          END { for(k in s) print k, s[k]/c[k] }     │
│                                                      │
│  COMBO:  Use MULTIPLE arrays with SAME key           │
│          max[$1]=$3; who[$1]=$2                       │
│                                                      │
│  PRINT:  END { for (k in arr) print k, arr[k] }     │
│                                                      │
│  5 QUESTIONS:                                        │
│    1. What is one line?                              │
│    2. What is the KEY?                               │
│    3. What is the VALUE?                             │
│    4. What OPERATION?                                │
│    5. When do I PRINT?                               │
│                                                      │
└──────────────────────────────────────────────────────┘
```

---

> 💡 **Final Exam Tip:** Every AWK array problem follows the same skeleton:
>
> ```awk
> { do_something[KEY] }
> END { for (k in do_something) print k, do_something[k] }
> ```
>
> The only thing that changes is the **operation**. Master the 4 patterns and you master AWK arrays.

---

*Made with ❤️ for exam success — using the Feynman technique.*
