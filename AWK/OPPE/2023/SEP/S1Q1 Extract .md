# 📘 Extract Docker Container Info (CSV using AWK)

---

## 📝 Problem Statement

You are given output of:

```bash
docker ps -a
```

Each line contains details about containers.

---

### 🎯 Your Task

Extract and print:

* CONTAINER ID
* IMAGE
* STATUS
* NAME

---

### ⚠️ Rules

* If STATUS contains **"Up"** → print `RUNNING`
* Otherwise → print `STOPPED`
* Output should be **CSV format**

---

## 📥 Sample Input

```text id="dk1"
CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS   NAMES
fa9130a84431   mongo-express   ...   Exited (143) 6 hours ago   ...   stock-analyser-mongo-express-1
830b9ccaa6c8   mongo   ...   Exited (0) 6 hours ago   ...   stock-analyser-mongo-1
db00571f20eb   redis   ...   Up 6 hours   ...   jovial_pasteur
```

---

## 📤 Sample Output

```text id="dk2"
CONTAINER ID,IMAGE,STATUS,NAMES
fa9130a84431,mongo-express,STOPPED,stock-analyser-mongo-express-1
830b9ccaa6c8,mongo,STOPPED,stock-analyser-mongo-1
db00571f20eb,redis,RUNNING,jovial_pasteur
```

---

## ❌ Issue in Your Code

```awk
$0 ~ "Exited"
```

👉 Problem:

* You are checking specific words
* But question says → **anything not "Up" = STOPPED**

---

# ✅ Simple & Correct Solution

```awk id="dk3"
#!/usr/bin/gawk -f

BEGIN {
    OFS=","
}

NR==1 {
    print "CONTAINER ID","IMAGE","STATUS","NAMES"
    next
}

{
    if ($0 ~ /Up/)
        status = "RUNNING"
    else
        status = "STOPPED"

    print $1, $2, status, $NF
}
```

---

# 🔍 Explanation (Simple)

---

### 🔹 Skip Header

```awk
NR==1
```

👉 Print your own header

---

### 🔹 Check Status

```awk
$0 ~ /Up/
```

👉 If line contains "Up" → RUNNING

---

### 🔹 Else

```awk
else → STOPPED
```

👉 Covers all other cases

---

### 🔹 Extract Fields

```awk
$1 → container id  
$2 → image  
$NF → name  
```

---

## 🎯 Key Idea

👉
**pattern match → condition → print fields**

---

## ⚠️ Important Points

* Do NOT rely only on "Exited"
* Always check "Up"
* `$NF` = last column

---

## 🧠 Memory Trick

👉
**Up → RUNNING
Else → STOPPED**

---


