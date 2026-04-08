# 📘 Find Highest Usage Service per Person

---

# Problem Statement

You are given input where each line represents:

```text
Name:Service^Value
```

Each record contains:

* **Name** → person name
* **Service** → service used
* **Value** → usage amount

---

### 🎯 Your Task

For each person:

* Sum values **per service**
* Find the **service with highest total usage**
* Print output in the format:

```text
Name, Service
```

---

# Sample Input

```text id="svc1"
Alice:nginx^12.5
Bob:redis^8.0
Alice:nginx^15.0
Alice:postgres^20.0
Bob:ngnix^10.0
Bob:redis^8.0
```

---

# Sample Output

```text id="svc2"
Alice, nginx
Bob, redis
```

---

# 🧠 Key Idea

👉 Each line has **2 separators**:

* `:`
* `^`

So we split using:

```bash id="svc3"
-F'[:^]'
```

---

# 🔹 Solution

```bash id="svc4"
awk -F'[:^]' '
{
    name = $1
    service = $2
    value = $3

    # accumulate value per (name, service)
    sum[name, service] += value

    # track maximum per person
    if (sum[name, service] > max[name]) {
        max[name] = sum[name, service]
        best[name] = service
    }
}

END {
    for (n in best) {
        print n ", " best[n]
    }
}
'
```

---

# 🔍 Explanation

---

## 🔹 Field Splitting

```awk id="svc5"
-F'[:^]'
```

👉 Splits:

```text
Alice:nginx^12.5
```

Into:

* `$1` → Alice
* `$2` → nginx
* `$3` → 12.5

---

## 🔹 Sum Storage

```awk id="svc6"
sum[name, service] += value
```

👉 Stores totals like:

```text
sum["Alice","nginx"] = 27.5
sum["Bob","redis"] = 16.0
```

---

## 🔹 Track Maximum

```awk id="svc7"
if (sum[name, service] > max[name])
```

👉 Keeps highest value per person

---

## 🔹 Store Best Service

```awk id="svc8"
best[name] = service
```

👉 Saves service with highest total

---

# 🧠 Step-by-Step Visualization

---

## 1️⃣ After Processing All Lines

### Sum Table

| Name  | Service  | Total |
| ----- | -------- | ----- |
| Alice | nginx    | 27.5  |
| Alice | postgres | 20.0  |
| Bob   | redis    | 16.0  |
| Bob   | nginx    | 10.0  |

---

## 2️⃣ Max Table

| Name  | Max Value |
| ----- | --------- |
| Alice | 27.5      |
| Bob   | 16.0      |

---

## 3️⃣ Best Service Table

| Name  | Best Service |
| ----- | ------------ |
| Alice | nginx        |
| Bob   | redis        |

---

# ⚡ Key Concept

👉 `sum[name, service]`
= separate storage for each combination

👉 `max[name]`
= best value per person

👉 `best[name]`
= corresponding service

---
