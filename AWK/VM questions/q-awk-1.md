

# Playlist Filter using AWK

---

# Problem Statement

You are the disk jockey at a local radio station preparing a playlist for a **New Year's Eve party**.

You are given a dataset of songs (`spotify.csv`). You must select songs based on:

* Songs released in the current year (`YEAR` environment variable)
* AND either:

  * The song is in **Major mode** and has **BPM ≥ 120**
  * OR the song has **danceability > 80%**

The output should preserve the original order of the dataset.

---

# Input Format

```csv
track_name,artist(s)_name,artist_count,released_year,released_month,released_day,in_spotify_playlists,in_spotify_charts,streams,in_apple_playlists,in_apple_charts,in_deezer_playlists,in_deezer_charts,in_shazam_charts,bpm,key,mode,danceability_%,valence_%,energy_%,acousticness_%,instrumentalness_%,liveness_%,speechiness_%
```

---

## Sample Input

```csv
Seven (feat. Latto) (Explicit Ver.),"Latto, Jung Kook",2,2023,7,14,553,147,141381703,43,263,45,10,826,125,B,Major,80,89,83,31,0,8,4
LALA,Myke Towers,1,2023,3,23,1474,48,133716286,48,126,58,14,382,92,C#,Major,71,61,74,7,0,10,4
vampire,Olivia Rodrigo,1,2023,6,30,1397,113,140003974,94,207,91,14,949,138,F,Major,51,32,53,17,0,31,6
Cruel Summer,Taylor Swift,1,2019,8,23,7858,100,800840817,116,207,125,12,548,170,A,Major,55,58,72,11,0,11,15
WHERE SHE GOES,Bad Bunny,1,2023,5,18,3133,50,303236322,84,133,87,15,425,144,A,Minor,65,23,80,14,63,11,6
Sprinter,"Dave, Central Cee",2,2023,6,1,2186,91,183706234,67,213,88,17,946,141,C#,Major,92,66,58,19,0,8,24
Ella Baila Sola,"Eslabon Armado, Peso Pluma",2,2023,3,16,3090,50,725980112,34,222,43,13,418,148,F,Minor,67,83,76,48,0,8,3
Columbia,Quevedo,1,2023,7,7,714,43,58149378,25,89,30,13,194,100,F,Major,67,26,71,37,0,11,4
fukumean,Gunna,1,2023,5,15,1096,83,95217315,60,210,48,11,953,130,C#,Minor,85,22,62,12,0,28,9
```

---

## Sample Output

```txt
Seven (feat. Latto) (Explicit Ver.) - "Latto, Jung Kook"
vampire - Olivia Rodrigo
Sprinter - "Dave, Central Cee"
fukumean - Gunna
```

---

# Solution

```bash
#!/bin/bash

awk '
BEGIN {
    FPAT = "([^,]+)|(\"[^\"]+\")"
    YEAR = ENVIRON["YEAR"]
}

{
    released_year = $4
    bpm           = $15
    mode          = $17
    dance         = $18

    if ((YEAR == released_year) && ((bpm >= 120 && mode == "Major") || (dance > 80)))
        print $1, "-", $2
}
'
```

---

# Explanation

* `FPAT` → Defines **what a field looks like** (instead of what separates fields)

### 🔍 Why not use `FS=","`?

Normally, AWK splits fields using:

```awk
FS = ","
```

But this fails for CSV like:

```text
"Latto, Jung Kook"
```

👉 It incorrectly splits into:

```text
"Latto   Jung Kook"
```

---

### ✅ How `FPAT` fixes this

```awk
FPAT = "([^,]+)|(\"[^\"]+\")"
```

This means:

* `([^,]+)` → match normal fields (no commas)
* `(\"[^\"]+\")` → match quoted fields (even if they contain commas)

👉 So this line:

```text
Sprinter,"Dave, Central Cee"
```

Is correctly parsed as:

* `$1 = Sprinter`
* `$2 = "Dave, Central Cee"`

---

### ⚡ Key Difference

| Feature         | FS         | FPAT    |
| --------------- | ---------- | ------- |
| Defines         | separators | fields  |
| CSV with quotes | ❌ breaks   | ✅ works |

---

### Other Logic

* `ENVIRON["YEAR"]` → Reads the year from environment
* `$4`, `$15`, `$17`, `$18` → Extract required columns

### Condition:

```awk
YEAR == released_year &&
(
  (mode == "Major" && bpm >= 120) ||
  (dance > 80)
)
```

---

## 🧠 Key Concept

👉 Use `FPAT` when working with **CSV files containing quoted fields with commas**.

👉 It ensures **accurate field extraction**, which is critical for correct filtering.

---

👉 Example run:

```bash
YEAR=2023 ./script.sh < spotify.csv
```

---

