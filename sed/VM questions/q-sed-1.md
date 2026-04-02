## ROT13 Decryption
---

# Problem Statement

You are given a spy message encrypted using the **ROT13 technique**.

Your task is to **decrypt the message** and print the original text.

The transformation must:

* Replace each letter with the **13th letter after it**
* Work for both **lowercase and uppercase letters**
* Leave **non-alphabet characters unchanged**

---

# Sample Input

```text
uryyb
```

---

# Sample Output

```text
hello
```

---

# Solution

```bash
sed 'y/abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ/nopqrstuvwxyzabcdefghijklmNOPQRSTUVWXYZABCDEFGHIJKLM/'
```

---

# Explanation

* `y///` → Transliteration command in sed (character-by-character replacement)

* First set:

  ```bash
  abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
  ```

  → original characters

* Second set:

  ```bash
  nopqrstuvwxyzabcdefghijklmNOPQRSTUVWXYZABCDEFGHIJKLM
  ```

  → mapped ROT13 characters

* Each character is replaced by its **corresponding position**

* Non-alphabet characters remain unchanged

---

## 🧠 Key Concept

👉 `y///` is used when:

* You want **character-by-character replacement**
* NOT pattern-based replacement

---


👉 Output:

```bash
hello
```

---
