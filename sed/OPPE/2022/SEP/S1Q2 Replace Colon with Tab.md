# Replace Colon with Tab (Except Escaped) using sed

---

## 📝 Problem Statement

Write a **sed script (`script.sed`)** that:

* Replaces all `:` with a **tab character (`\t`)**
* But if `:` is preceded by `\:`:

  * It should be replaced with `:` (i.e., remove backslash and keep colon)

---

## 📥 Sample Input 1

```text id="c1"
a:b:c
```

---

## 📤 Sample Output 1

```text id="c2"
a	b	c
```

---

## 📥 Sample Input 2

```text id="c3"
a\:b:c
```

---

## 📤 Sample Output 2

```text id="c4"
a:b	c
```

---

## 📥 Sample Input 3

```text id="c5"
:
```

---

## 📤 Sample Output 3

```text id="c6"
	
```

---

## 📥 Sample Input 4

```text id="c7"
\:
```

---

## 📤 Sample Output 4

```text id="c8"
:
```

---

## 📥 Sample Input 5

```text id="c9"
1:2:3:4:5
1:2:3:4:5
1:2:3:4:5
1:2:3:4:5
1:2:3:4:5
```

---

## 📤 Sample Output 5

```text id="c10"
1	2	3	4	5
1	2	3	4	5
1	2	3	4	5
1	2	3	4	5
1	2	3	4	5
```

---

## 📥 Sample Input 6

```text id="c11"
\::\::\:
```

---

## 📤 Sample Output 6

```text id="c12"
:	:	:
```

---

## ✅ Solution — CLI

```bash id="c13"
sed '
s/\\:/__TEMP__/g
s/:/\t/g
s/__TEMP__/:/g
' input.txt
```

---

## ✅ Solution — script.sed

```sed id="c14"
s/\\:/__TEMP__/g
s/:/\t/g
s/__TEMP__/:/g
```

Run using:

```bash id="c15"
sed -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Step 1: Protect Escaped Colon

```bash id="c16"
s/\\:/__TEMP__/g
```

* Replaces `\:` with a placeholder
* Prevents it from being converted to tab

---

### 🔹 Step 2: Replace Remaining Colon with Tab

```bash id="c17"
s/:/\t/g
```

* Converts all remaining `:` → tab

---

### 🔹 Step 3: Restore Escaped Colon

```bash id="c18"
s/__TEMP__/:/g
```

* Converts placeholder back to `:`

---

## 🎯 Key Idea

👉 **Protect → Replace → Restore**

---

## ⚠️ Important Points

* Order of commands is critical
* `\\:` → escaped colon
* `\t` → tab character
* Placeholder must be unique

---

## 🧠 Memory Trick

👉 **Escaped safe → normal replace → restore original**

---
