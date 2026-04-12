# Extract Email Domain using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Reads input containing records with email addresses
* Extracts and prints only the **domain name** from each email
* The domain name is everything **after `@`**
* Ignore all other fields

---

## 📥 Sample Input

```text id="em1"
Mehta,1-322-248-9102,mehta@yahoo.net,Daman and Diu
Malik,1-335-746-5592,malik@hotmail.com,Andhra Pradesh
Suri,1-134-516-1754,suri@protonmail.org,Lakshadweep
Persaud,268-1152,persaud302@aol.co.uk,Dadra and Nagar Haveli
Joshi,1-869-326-6582,joshi4288@hotmail.com,OR
Subram,1-172-433-4318,subram@hotmail.edu,MH
Sehgal,647-1701,sehgal@outlook.co.uk,DD
```

---

## 📤 Sample Output

```text id="em2"
yahoo.net
hotmail.com
protonmail.org
aol.co.uk
hotmail.com
hotmail.edu
outlook.co.uk
```

---

## 💻 Solution (Command Line)

```bash id="em3"
sed -E 's/.*@//; s/,.*//' 
```

---

## 💻 Solution (script.sed)

```sed id="em4"
s/.*@//
s/,.*//
```

Run using:

```bash id="em5"
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Remove Everything Before `@`

```bash id="em6"
s/.*@//
```

* `.*` → any characters
* `@` → match until `@`
* Removes everything before and including `@`

---

### 🔹 Remove Everything After Domain

```bash id="em7"
s/,.*//
```

* `,` → start of next field
* `.*` → everything after
* Keeps only the domain

---

## 🎯 Key Idea

👉 Extract:

* After `@`
* Before next `,`

---

## ⚠️ Important Points

* Works for multiple domain formats (`.com`, `.org`, `.co.uk`)
* Order of substitutions matters
* No need for `-n`

---

## 🧠 Memory Trick

👉 **Cut before @ → cut after comma → domain remains**
