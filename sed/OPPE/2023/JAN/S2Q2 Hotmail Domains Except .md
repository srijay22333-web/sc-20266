# Filter Hotmail Domains (Except hotmail.com) using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Reads records containing email addresses
* Prints only the lines where the email domain contains **"hotmail"**
* Excludes lines where the domain is exactly **"hotmail.com"**

---

## 📥 Sample Input

```text id="hm1"
name,phone,email,region
Sahni,151-8534,sahni@hotmail.ca,KA
Mehta,1-322-248-9102,mehta@yahoo.net,Daman and Diu
Malik,1-335-746-5592,malik@hotmail.com,Andhra Pradesh
Suri,1-134-516-1754,suri@protonmail.org,Lakshadweep
Persaud,268-1152,persaud302@aol.co.uk,Dadra and Nagar Haveli
Joshi,1-869-326-6582,joshi4288@hotmail.com,OR
Subram,1-172-433-4318,subram@hotmail.edu,MH
```

---

## 📤 Sample Output

```text id="hm2"
Sahni,151-8534,sahni@hotmail.ca,KA
Subram,1-172-433-4318,subram@hotmail.edu,MH
```

---

## 💻 Solution (Command Line)

```bash id="hm3"
sed '/hotmail/ {/hotmail\.com/d}; /hotmail/!d'
```

---

## 💻 Solution (script.sed)

```sed id="hm4"
/hotmail/ {
/hotmail\.com/d
}
/hotmail/!d
```

Run using:

```bash id="hm5"
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Match "hotmail"

```bash id="hm6"
/hotmail/
```

* Selects lines containing `"hotmail"`

---

### 🔹 Exclude hotmail.com

```bash id="hm7"
/hotmail\.com/d
```

* Deletes lines where domain is exactly `"hotmail.com"`

---

### 🔹 Remove non-hotmail lines

```bash id="hm8"
/hotmail/!d
```

* Deletes lines that do **not contain "hotmail"**

---

## 🎯 Key Idea

👉 Combine:

* Pattern match
* Conditional block `{}`
* Deletion (`d`)

---

## ⚠️ Important Points

* `\.` is used to match literal `.`
* Order matters: filter → exclude → delete others
* Case-sensitive matching

---

## 🧠 Memory Trick

👉 **Keep hotmail → remove hotmail.com → delete rest**
