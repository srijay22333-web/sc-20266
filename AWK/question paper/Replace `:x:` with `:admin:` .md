# 📘 Replace `:x:` with `:admin:` (Without Modifying File)

---

## 📝 Problem Statement

Given a file `users.txt`, replace `:x:` with `:admin:` in each line **without modifying the original file**.

---

## 🎯 Task

* Replace the **second field (`x`)** with `admin`
* Keep the rest of the line unchanged
* Print the result to output (do not edit the file)

---

## 📥 Sample Input

```text id="q7t2ix"
alice:x:1001
bob:x:1002
Charlie:x:1003
```

---

## 📤 Sample Output

```text id="paj2yt"
alice:admin:1001
bob:admin:1002
Charlie:admin:1003
```

---

## ✅ Solution — script.awk

```awk id="y6rlmx"
BEGIN {
    FS = ":"
    OFS = ":"
}

{
    $2 = "admin"
    print $0
}
```

---

## ▶️ Run

```bash id="x5nqcl"
awk -f script.awk users.txt
```

---

## 🔍 Explanation

* `FS = ":"` → splits each line into fields using `:`
* `OFS = ":"` → ensures output is joined using `:`
* `$2 = "admin"` → replaces the second field
* `print $0` → prints the modified full line

---

## 🧠 Visual Understanding

### ▶ Original Line

```text id="z3v4cz"
alice:x:1001
```

Split into fields:

```text id="9q0qk7"
$1 = alice
$2 = x
$3 = 1001
```

After modification:

```text id="4uk4l0"
$2 = admin
```

Reconstructed output:

```text id="c8r7fs"
alice:admin:1001
```

---

## 🔥 Key Concept

```text id="bq8kxu"
Modify field → AWK rebuilds full line using OFS
```
