# IPv4 Address Regex

---

## 📝 Problem Statement

Match IPv4 addresses

---

## ✅ Solution (ERE)

```regex
^([0-9]{1,3}\.){3}[0-9]{1,3}$
```

---

## 🔍 Explanation

* 4 number groups
* separated by `.`

---

---

# Replace on Even Lines Only

---

## 📝 Problem Statement

Replace `"apple"` with `"orange"` only on even lines

---

## 📥 Sample Input

```text
apple
apple
apple
apple
```

---

## 📤 Sample Output

```text
apple
orange
apple
orange
```

---

## ✅ Solution — CLI

```bash
sed '2~2s/apple/orange/g' fruits.txt
```

---

## ✅ Solution — script.sed

```sed
2~2s/apple/orange/g
```

---

## 🔍 Explanation

* `2~2` → every 2nd line

---
