# Remove Lines with Specific Words using sed

---

## 📝 Problem Statement

Write a **sed script** that:

* Removes all lines containing any of the words:

  * `install`
  * `installed`
  * `status`

* Prints only the remaining lines

---

## 📥 Sample Input

```text id="rm1"
2022-05-05 15:55:18 purge grub-pc:amd64 2.02-2ubuntu8.23 <none>
2022-05-05 15:55:18 purge grub-legacy-ec2:all 1:1 <none>
2022-05-05 15:55:18 purge grub-common:amd64 2.02-2ubuntu8.23 <none>
2022-05-05 15:55:14 status installed linux-virtual:amd64 4.15.0.176.165
2022-05-05 15:55:14 status half-installed linux-virtual:amd64 4.15.0.176.165
2022-05-05 15:55:14 status not-installed linux-virtual:amd64 <none>
2022-05-05 15:55:17 status installed grub-legacy-ec2:all 1:1
2022-05-05 15:55:17 status half-installed grub-legacy-ec2:all 1:1
2022-05-05 15:55:17 status installed os-prober:amd64 1.74ubuntu1
2022-05-05 15:55:17 status half-installed os-prober:amd64 1.74ubuntu1
2022-05-05 15:55:17 status installed grub2-common:amd64 2.02-2ubuntu8.23
2022-05-05 16:13:44 status half-installed libogg0:amd64 1.3.2-1
2022-05-05 16:13:44 install x11-common:all <none> 1:7.7+19ubuntu7.1
2022-05-05 16:13:44 status half-installed x11-common:all 1:7.7+19ubuntu7.1
2022-05-05 16:13:44 install libice6:amd64 <none> 2:1.0.9-2
2022-05-05 16:13:44 status half-installed libice6:amd64 2:1.0.9-2
2022-05-05 16:13:44 install libsm6:amd64 <none> 2:1.2.2-1
2022-05-05 16:13:44 status half-installed libsm6:amd64 2:1.2.2-1
2022-05-05 16:13:44 install fonts-dejavu-core:all <none> 2.37-1
2022-05-05 16:13:44 status half-installed fonts-dejavu-core:all 2.37-1
2022-05-05 16:13:44 install fontconfig-config:all <none> 2.12.6-0ubuntu2
2022-05-05 16:13:44 status half-installed fontconfig-config:all 2.12.6-0ubuntu2
2022-05-05 16:13:44 install libfontconfig1:amd64 <none> 2.12.6-0ubuntu2
2022-05-05 16:13:44 status half-installed libfontconfig1:amd64 2.12.6-0ubuntu2
2022-05-05 16:13:44 install libxrender1:amd64 <none> 1:0.9.10-1
```

---

## 📤 Sample Output

```text id="rm2"
2022-05-05 15:55:18 purge grub-pc:amd64 2.02-2ubuntu8.23 <none>
2022-05-05 15:55:18 purge grub-legacy-ec2:all 1:1 <none>
2022-05-05 15:55:18 purge grub-common:amd64 2.02-2ubuntu8.23 <none>
```

---

## 💻 Solution 1 (Using ERE)

```bash id="rm3"
sed -E '/install|installed|status/d'
```

---

## 💻 Solution 2 (Simple Multiple Rules)

```bash id="rm4"
sed '
/install/d
/installed/d
/status/d
'
```

---

## 🔍 Explanation

### 🔹 Solution 1

```bash id="rm5"
/install|installed|status/
```

* Matches any line containing:

  * `install` OR `installed` OR `status`

👉 `d` deletes matching lines

---

### 🔹 Solution 2

```bash id="rm6"
/install/d
/installed/d
/status/d
```

* Deletes lines matching each pattern separately

---

## 🎯 Key Idea

👉 Delete lines containing unwanted patterns

---

## ⚠️ Important Points

* Order does not matter here
* `-E` enables use of `|` (OR operator)
* Substring match: `install` also matches `installed`

---

## 🧠 Memory Trick

👉 **Match unwanted → delete → keep rest**
