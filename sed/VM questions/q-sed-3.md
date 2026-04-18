# Pretty Print JSON Data Object using sed

---

## 📝 Problem Statement

You are given a **minified JSON string** as input.

Your task is to:

* Extract only the **`data` object**
* Pretty print it in readable format

---

### 🎯 Requirements

* Remove outer `{ "data": ... }`
* Add proper **line breaks and indentation**
* Assume:

  * No nested JSON objects (basic version)

---

## 📥 Sample Input

```json id="j1"
{ "data": { "name": "John", "age": 30, "city": "New York" } }
```

---

## 📤 Sample Output

```json id="j2"
{
	"name": "John",
	"age": 30,
	"city": "New York"
}
```

---

## 💻 Solution (Script Format)

```sed id="j3"
s/{ "data": //
s/}$//
s/{/{\n\t/g
s/}/\n}/g
s/,/,\n\t/g
```

---

## 🔍 Explanation

### 🔹 Remove Wrapper (`data` key)

```sed id="j4"
s/{ "data": //
```

* Removes outer structure:

  ```
  { "data": { ... } }
  ```

  → becomes:

  ```
  { ... }
  ```

---

### 🔹 Remove Last Closing Brace

```sed id="j5"
s/}$//
```

* Removes extra `}` from outer JSON

---

### 🔹 Add Newline after `{`

```sed id="j6"
s/{/{\n\t/g
```

* Starts formatting:

  ```
  {
  	...
  }
  ```

---

### 🔹 Format Closing Brace

```sed id="j7"
s/}/\n}/g
```

* Moves `}` to new line

---

### 🔹 Add Newline after Commas

```sed id="j8"
s/,/,\n\t/g
```

* Each key-value pair moves to new line

---

## 🧠 Key Concept

👉 This uses:

* **Substitution (`s///`)**
* **Text formatting with `\n` and `\t`**
* **Pattern cleanup**

---

## ⚠️ Important Note

* Works only for:

  * **Flat JSON (no nested objects)**
* Not reliable for complex JSON

---

## 🚀 Bonus Insight (Nested JSON)

👉 sed becomes messy for nested JSON

Better tool:

```bash id="j9"
jq '.data'
```

✔ Handles nested structures
✔ Proper formatting automatically

---

## 🧠 Memory Trick

👉 **Remove wrapper → add newlines → indent → done**

---
