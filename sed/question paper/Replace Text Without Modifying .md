# Replace Text Without Modifying File

---

## 📝 Problem Statement

Write a **sed command** that:

* Replaces `:x:` with `:admin:`
* Does **NOT modify original file**

---

## 📥 Sample Input

```text
alice:x:1001
bob:x:1002
Charlie:x:1003
```

---

## 📤 Sample Output

```text
alice:admin:1001
bob:admin:1002
Charlie:admin:1003
```

---

## ✅ Solution — CLI

```bash
sed 's/:x:/:admin:/' users.txt
```

---

## ✅ Solution — script.sed

```sed
s/:x:/:admin:/
```

```bash
sed -f script.sed users.txt
```

---

## 🔍 Explanation

* `s/:x:/:admin:/` → replaces first occurrence per line
* No `-i` → file is unchanged

---


