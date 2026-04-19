# 📘 Word Frequency Counter

---

## 📝 Problem Statement

Count the frequency of each word in a file while:

👉 Ignoring **case differences** and **special characters**

---

## 🎯 Task

* Convert all words to lowercase
* Remove symbols and punctuation
* Count occurrences of each word
* Print word frequency

---

## 📥 Sample Input

```text id="wf_in"
Hello world!
Hello, AWK world.
AWK is powerful; awk is fast.
```

---

## 📤 Sample Output

```text id="wf_out"
hello           2
world           2
awk             3
is              2
powerful        1
fast            1
```

---

## ⚠️ Important Note

* `tolower()` ensures **case-insensitive comparison**
* `gsub(/[^a-z]/, "", word)` removes **non-alphabet characters**
* Empty strings are ignored

---

## ✅ Solution — script.awk

```awk id="awk_word_freq"
{
    for (i = 1; i <= NF; i++) {
        word = tolower($i)
        gsub(/[^a-z]/, "", word)

        if (word != "")
            freq[word]++
    }
}

END {
    for (word in freq) {
        printf "%-15s %d\n", word, freq[word]
    }
}
```

---

## ▶️ Run

```bash id="run_wf"
awk -f script.awk input.txt
```

---

## 🔍 Explanation

* `NF` → number of fields (words in a line)
* Loop iterates through each word
* `tolower($i)` → converts word to lowercase
* `gsub(/[^a-z]/, "", word)` → removes punctuation/symbols
* `freq[word]++` → increments word count
* `printf "%-15s %d\n"` → formatted aligned output

---

## 🧠 Visual Understanding

### ▶ Normalization Phase

```text id="wf_norm"
Hello   → hello
Hello,  → hello
AWK     → awk
awk     → awk
```

---

### ▶ Counting Phase

```text id="wf_count"
hello → 2
world → 2
awk   → 3
is    → 2
```

---

## 🔥 Key Concept

```text id="wf_key"
gsub(/[^a-z]/, "", word) → remove non-letter characters
```

---

## ✅ Alternative (Sorted Output)

```awk id="wf_alt"
{
    for (i = 1; i <= NF; i++) {
        word = tolower($i)
        gsub(/[^a-z]/, "", word)
        if (word != "")
            freq[word]++
    }
}

END {
    for (word in freq) {
        print word, freq[word]
    }
}
```


