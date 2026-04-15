# Join Lines Ending with Backslash

---

## 📝 Problem Statement

Write a **sed script** that:

* Joins lines ending with `\` with the next line

---

## 📥 Sample Input

```text
Hello \
World
This is \
sed
```

---

## 📤 Sample Output

```text
Hello World
This is sed
```

---

## ✅ Solution — CLI

```bash
sed ':a; /\\$/N; s/\\\n//; ta' file.txt
```

---

## ✅ Solution — script.sed

```sed
:a
/\\$/N
s/\\\n//
ta
```

```bash
sed -f script.sed file.txt
```

---

## 🔍 Explanation

* `:a` → label
* `/\\$/` → line ends with `\`
* `N` → append next line
* `s/\\\n//` → remove backslash + newline
* `ta` → loop

---



If you want next: I can compress this into a **1-page exam cheat sheet (top 20 sed patterns)** 👍
