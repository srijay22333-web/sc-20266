#  Convert Case Based on Prefix

---

## 📝 Problem

Each input line is in format:

```bash
case:text
```

👉 If `case = low` → convert `text` to **lowercase**
👉 If `case = up` → convert `text` to **uppercase**
👉 Print only the **text part after conversion**

---

## 📥 Input Example

```bash
low:This is a Low Case Message
up:This is an Up Case Message
```

---

## 📤 Output Example

```bash
this is a low case message
THIS IS AN UP CASE MESSAGE
```

---

## 💻 Solution Code (Simple)

```bash
sed -E 's/^low:(.*)/\L\1/; s/^up:(.*)/\U\1/'
```

---

## 🔍 Explanation

### 1. `^low:(.*)`

* `^` → start of line
* `low:` → match lines starting with "low:"
* `(.*)` → capture the text part

---

### 2. `\L\1`

* `\1` → captured text
* `\L` → convert to lowercase

---

### 3. `^up:(.*)`

* Matches lines starting with `"up:"`

---

### 4. `\U\1`

* `\U` → convert to uppercase

---

### 5. `;` (Multiple commands)

👉 Runs both substitutions in one command

---

## 🎯 Key Idea

* Use **pattern addressing (`^low`, `^up`)**
* Use **capture group `(.*)`**
* Use:

  * `\L` → lowercase
  * `\U` → uppercase

---

## ⚠️ Important Exam Points

* `^` → start of line (very important)
* `(.*)` → capture everything after `:`
* `\1` → reuse captured text
* `-E` → enables extended regex

---

## 🚀 Quick Test

```bash
echo "low:Hello World" | sed -E 's/^low:(.*)/\L\1/'
```

👉 Output:

```bash
hello world
```

---

## 🧠 One-Line Memory Trick

👉 **Match → Capture → Convert → Print**

---



