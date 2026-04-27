# 📘 Count Direct Subordinates (AWK)

---

## 📝 Problem Statement

You are given a **CSV file** where each row contains:

```csv
id,name,manager_id
```

---

### 📌 Field Meaning

* `id` → employee ID
* `name` → employee name
* `manager_id` → ID of their manager

  * `0` means no manager

---

## 🎯 Your Task

For each employee:

* Count how many employees report **directly** to them
* Print output in format:

```csv
id,name,direct_subordinates
```

---

## 📥 Sample Input

```text id="emp1"
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

```text id="emp2"
1,Eve,3
2,Hannah,0
3,Eve,0
4,Bob,0
5,Grace,1
6,Eve,0
7,Olivia,0
```

---

# 💻 Solution (script.awk)

```awk id="emp3"
#!/usr/bin/gawk -f

BEGIN {
    FS=","
    OFS=","
}

{
    manages[$3]++      # count how many report to manager_id
    name[$1] = $2      # store id → name
}

END {
    for (i in name) {
        print i, name[i], manages[i] + 0
    }
}
```

---

# 🔍 Explanation (Simple)

---

## 🔹 Count Subordinates

```awk id="emp4"
manages[$3]++
```

👉 `$3` = manager_id
👉 Count how many employees report to each manager

---

## 🔹 Store Name Mapping

```awk id="emp5"
name[$1] = $2
```

👉 `$1` = id
👉 `$2` = name

---

## 🔹 Print Result

```awk id="emp6"
print i, name[i], manages[i] + 0
```

👉 `manages[i] + 0` ensures:

* prints `0` if no subordinates

---

# 🎯 Key Idea

👉
**manager_id → count reports**

---

# ⚠️ Important Points

* No header → process all lines
* Use arrays:

  * `manages[]` → counts
  * `name[]` → mapping
* Output order may vary

---

# 🧠 Memory Trick

👉
**who manages → count
who is → name**

---

# 🚀 Final Insight

This is a **mapping + counting problem**:

```text
group by manager → count → print with id
```
