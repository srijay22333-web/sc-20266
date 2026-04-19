# 📘 Count IP Occurrences (> 2)

---

## 📝 Problem Statement

Given a file `log.txt`, each line contains:

```
IP_Address Request_Type
```

Count how many times each IP appears and print only those with occurrences greater than **2**.

---

## 🎯 Task

* Count occurrences of each IP address
* Filter and print only IPs with count **> 2**

---

## 📥 Sample Input

```text id="ipin01"
192.168.0.1 GET
192.168.0.2 GET
192.168.0.1 POST
192.168.0.3 GET
192.168.0.1 PUT
192.168.0.2 GET
```

---

## 📤 Sample Output

```text id="ipout01"
192.168.0.1 3
```

---

## ⚠️ Important Note

* `$1` refers to the **IP address**
* `$2` (request type) is ignored for counting
* Associative arrays in AWK make counting efficient

---

## ✅ Solution — script.awk

```awk id="awk_ip_count"
{
    count[$1]++
}

END {
    for (ip in count) {
        if (count[ip] > 2) {
            print ip, count[ip]
        }
    }
}
```

---

## ▶️ Run

```bash id="run_ip_count"
awk -f script.awk log.txt
```

---

## 🔍 Explanation

* `count[$1]++` → increments count for each IP
* `for (ip in count)` → loops through all IPs
* `if (count[ip] > 2)` → filters required condition
* `print ip, count[ip]` → displays result

---

## 🧠 Visual Understanding

### ▶ Counting Phase

```text id="count_phase"
192.168.0.1 → 1 → 2 → 3
192.168.0.2 → 1 → 2
192.168.0.3 → 1
```

---

### ▶ Filtering (>2)

```text id="filter_phase"
192.168.0.1 → 3 ✅
```

---

## 🔥 Key Concept

```text id="key_concept_ip"
count[$1]++  → frequency counting using associative arrays
```

---

