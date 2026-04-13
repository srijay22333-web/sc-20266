# Extract TODO Comments using sed (ERE)

---

## 📝 Problem Statement

Write a **sed script** that:

* Searches for lines containing `# TODO:`
* Extracts only the **task part** after `# TODO:`
* Prints only the extracted content

---

## 📥 Sample Input

```text id="td1"
...
        if self.propagate_exceptions:
            # if we want to repropagate the exception, we can attempt to
            # raise it with the whole traceback in case we can do that
            # (the function was actually called from the except part)
            # otherwise, we just raise the error again
            if exc_value is e:
                reraise(exc_type, exc_value, tb)
            else:
                raise e

        self.log_exception((exc_type, exc_value, tb))
        server_error = InternalServerError()
        # TODO: pass as param when Werkzeug>=1.0.0 is required
        # TODO: also remove note about this from docstring and docs
        server_error.original_exception = e
        handler = self._find_error_handler(server_error)
...
```

---

## 📤 Sample Output

```text id="td2"
pass as param when Werkzeug>=1.0.0 is required
also remove note about this from docstring and docs
```

---

## 💻 Solution (Command Line - ERE)

```bash id="td3"
sed -E '/# TODO:/!d; s/.*# TODO: //'
```

---

## 💻 Solution (script.sed - ERE)

```sed id="td4"
/# TODO:/!d
s/.*# TODO: //
```

Run using:

```bash id="td5"
sed -E -f script.sed input.txt
```

---

## 🔍 Explanation

### 🔹 Filter TODO Lines

```bash id="td6"
/# TODO:/!d
```

* Keeps only lines containing `# TODO:`
* Deletes all other lines

---

### 🔹 Extract Task Text

```bash id="td7"
s/.*# TODO: //
```

* Removes everything before `# TODO:`
* Keeps only the task description

---

## 🎯 Key Idea

👉 Filter → Extract

---

## ⚠️ Important Points

* Order matters: first filter, then replace
* `-E` is optional here (no complex regex used)
* Case-sensitive match

---

## 🧠 Memory Trick

👉 **Keep TODO → remove prefix → print task**

