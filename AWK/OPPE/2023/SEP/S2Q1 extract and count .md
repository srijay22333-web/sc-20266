# 📘 Process dpkg Log (Installed & Removed Packages)

---

## 📝 Problem Statement

You are given a **dpkg log file** where each line contains:

* Date, time
* Action (`install`, `remove`, `purge`, etc.)
* Package details

---

### 🎯 Your Task

* Identify:

  * **INSTALLED packages**
  * **REMOVED packages**
* Print:

  * package name
  * version
  * status (`INSTALLED` / `REMOVED`)

---

### 📌 Rules

* Use **column 3** to determine action
* Count:

  * total installed
  * total removed

---

## 📥 Sample Input

```text id="dp1"
2022-05-05 15:55:14 remove linux-virtual:amd64 4.15.0.176.165 <none>
2022-05-05 16:13:44 install x11-common:all <none> 1:7.7+19ubuntu7.1
```

---

## 📤 Sample Output

```text id="dp2"
linux-virtual:amd64 <none> REMOVED
x11-common:all 1:7.7+19ubuntu7.1 INSTALLED
total installed=1; total removed=1
```

---

# ✅ Simple & Clean Solution (script.awk)

```awk id="dp3"
#!/usr/bin/gawk -f

{
    if ($3 == "install") {
        print $4, $6, "INSTALLED"
        icount++
    }
    else if ($3 == "remove") {
        print $4, $6, "REMOVED"
        rcount++
    }
}

END {
    print "total installed=" icount "; total removed=" rcount
}
```

---

# 🔍 Explanation (Simple)

---

### 🔹 Check Action

```awk id="dp4"
$3 == "install"
```

👉 Column 3 decides action

---

### 🔹 Installed

```awk id="dp5"
print $4, $6, "INSTALLED"
```

👉

* `$4` → package
* `$6` → version

---

### 🔹 Removed

```awk id="dp6"
$3 == "remove"
```

👉 print as REMOVED

---

### 🔹 Counting

```awk id="dp7"
icount++, rcount++
```

👉 track totals

---

### 🔹 Final Output

```awk id="dp8"
END
```

👉 print summary

---

# 🎯 Key Idea

👉
**Check column → classify → print → count**

---

# ⚠️ Important Points

* Ignore `status` lines
* Only use:

  * `install`
  * `remove`
* `$6` works because format is fixed

---

# 🧠 Memory Trick

👉
**$3 → action
$4 → package
$6 → version**

---

# 🚀 Final Insight

This is a **filter + count pattern**:

```text id="dp9"
match → print → count → summary
```
