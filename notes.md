# Python CSV Module
### Reading & Writing Data Like a Pro

**IT Fundamentals — Data Handling Unit**

---

## What is a CSV File?

**CSV = Comma-Separated Values**

- A plain text file that stores **tabular data**
- Each line is a **row**
- Values in each row are separated by **commas**
- The first row is usually a **header**

> Think of it as a spreadsheet saved as plain text.

---

## What Does a CSV Look Like?

**students.csv**

```
name,age,grade
Alice,18,90
Bob,19,85
Carol,18,92
Diana,20,78
```

- Line 1 → **Header row** (column names)
- Lines 2–5 → **Data rows**
- Each value is separated by a **comma**

---

## Where Do We See CSV in Real Life?

| Use Case | Example |
|---|---|
| 📊 Spreadsheets | Exported Excel/Google Sheets |
| 🏫 School Records | Student grade lists |
| 🛒 E-commerce | Product catalogs |
| 🏥 Health | Patient records |
| 🌐 Open Data | Government datasets |

CSV is one of the most common data formats in the world.

---

## 🎮 GAME — Spot the Valid CSV!

**Which of these is a properly formatted CSV?**

> Vote by raising your hand for A, B, or C!

**A.**
```
name | age | grade
Alice | 18 | 90
```

**B.**
```
name,age,grade
Alice,18,90
Bob,19,85
```

**C.**
```
name=Alice age=18 grade=90
name=Bob age=19 grade=85
```

---

## 🎮 ANSWER — Spot the Valid CSV!

✅ **B is correct!**

```
name,age,grade    ← header row, comma-separated
Alice,18,90       ← data rows, same structure
Bob,19,85
```

❌ **A** uses pipes `|` not commas — that's a different format (TSV-like)

❌ **C** uses `key=value` pairs — that's not CSV at all

---

## The Python `csv` Module

Python has a **built-in module** just for CSV files.

```python
import csv
```

No installation needed — it's part of Python's **standard library**.

**What can it do?**

- 📖 **Read** data from CSV files
- ✏️ **Write** data to CSV files
- 🛡️ Handle tricky cases (commas inside values, quotes, etc.)

---

## Opening a CSV File

Always use `open()` with the right arguments:

```python
with open('students.csv', 'r', newline='') as file:
    # do something here
```

| Argument | Purpose |
|---|---|
| `'students.csv'` | File name (or full path) |
| `'r'` | Mode: **r**ead |
| `newline=''` | Prevents extra blank lines (important on Windows!) |

> 💡 Always use `with open(...)` — it automatically closes the file for you.

---

## `csv.reader` — Reading Rows

`csv.reader` reads a CSV file **one row at a time** as a **list**.

```python
import csv

with open('students.csv', 'r', newline='') as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)
```

**Output:**
```
['name', 'age', 'grade']
['Alice', '18', '90']
['Bob', '19', '85']
['Carol', '18', '92']
```

Each row is a **Python list** of strings.

---

## Skipping the Header Row

The header row is usually not data — skip it with `next()`:

```python
import csv

with open('students.csv', 'r', newline='') as file:
    reader = csv.reader(file)
    next(reader)          # ← skip the header row
    for row in reader:
        name  = row[0]
        age   = row[1]
        grade = row[2]
        print(f"{name} is {age} years old with grade {grade}.")
```

**Output:**
```
Alice is 18 years old with grade 90.
Bob is 19 years old with grade 85.
Carol is 18 years old with grade 92.
```

---

## 🎮 GAME — Predict the Output!

**Given this CSV file (`scores.csv`):**
```
player,score
Rey,150
Sam,200
```

**And this code:**
```python
import csv
with open('scores.csv', 'r', newline='') as f:
    reader = csv.reader(f)
    next(reader)
    for row in reader:
        print(row[0], "scored", row[1])
```

**What will be printed? (30 seconds — write your answer!)**

---

## 🎮 ANSWER — Predict the Output!

✅ **Correct output:**

```
Rey scored 150
Sam scored 200
```

**Why?**
- `next(reader)` skips `['player', 'score']`
- First iteration: `row = ['Rey', '150']` → `row[0]` = `'Rey'`, `row[1]` = `'150'`
- Second iteration: `row = ['Sam', '200']`

> ⚠️ Note: all values are **strings** — even numbers!
> Use `int(row[1])` to convert to integer.

---

## `csv.DictReader` — Smarter Reading

`DictReader` reads each row as a **dictionary** instead of a list.

- Keys come from the **header row** automatically
- No need for `row[0]`, `row[1]` — use **column names** instead!

```python
import csv

with open('students.csv', 'r', newline='') as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(row['name'], "—", row['grade'])
```

**Output:**
```
Alice — 90
Bob — 85
Carol — 92
```

Much more **readable** than `row[0]`, `row[2]`! 🎉

---

## `csv.reader` vs `csv.DictReader`

| Feature | `csv.reader` | `csv.DictReader` |
|---|---|---|
| Row type | `list` | `dict` |
| Access by | Index `row[0]` | Key `row['name']` |
| Handles header | Manually (`next()`) | Automatically |
| Best for | Simple / small files | Named columns |

> 💡 When in doubt, use `DictReader` — your code will be easier to read and maintain.

---

## `csv.writer` — Writing Rows

Use `csv.writer` to create or write to a CSV file.

```python
import csv

with open('output.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerow(['name', 'age', 'grade'])   # header
    writer.writerow(['Alice', 18, 90])
    writer.writerow(['Bob', 19, 85])
```

**Result — output.csv:**
```
name,age,grade
Alice,18,90
Bob,19,85
```

- `'w'` mode **overwrites** the file (use `'a'` to append)
- `writerow()` writes **one row** at a time

---

## Writing Multiple Rows at Once

Use `writerows()` to write a **list of rows** in one call:

```python
import csv

students = [
    ['Alice', 18, 90],
    ['Bob', 19, 85],
    ['Carol', 18, 92],
]

with open('output.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerow(['name', 'age', 'grade'])
    writer.writerows(students)       # ← writes all rows at once
```

| Method | Writes |
|---|---|
| `writerow(row)` | One row (a list) |
| `writerows(rows)` | Many rows (a list of lists) |

---

## 🎮 GAME — Fill in the Blank!

**Complete the code to write this CSV:**

```
subject,score
Math,95
Science,88
English,91
```

```python
import csv

data = [
    ['Math', 95],
    ['Science', 88],
    ['English', 91],
]

with open('grades.csv', '___', newline='') as file:
    writer = csv.___(file)
    writer.___([ 'subject', 'score' ])
    writer.___(data)
```

**Fill in the 4 blanks! (60 seconds)**

---

## 🎮 ANSWER — Fill in the Blank!

```python
import csv

data = [
    ['Math', 95],
    ['Science', 88],
    ['English', 91],
]

with open('grades.csv', 'w', newline='') as file:      # ← 'w'
    writer = csv.writer(file)                           # ← writer
    writer.writerow(['subject', 'score'])               # ← writerow
    writer.writerows(data)                              # ← writerows
```

✅ `'w'` — write mode
✅ `csv.writer` — creates a writer object
✅ `writerow` — writes the single header row
✅ `writerows` — writes all data rows at once

---

## `csv.DictWriter` — Writing with Column Names

`DictWriter` lets you write rows as **dictionaries**.

```python
import csv

students = [
    {'name': 'Alice', 'age': 18, 'grade': 90},
    {'name': 'Bob',   'age': 19, 'grade': 85},
]

fields = ['name', 'age', 'grade']

with open('output.csv', 'w', newline='') as file:
    writer = csv.DictWriter(file, fieldnames=fields)
    writer.writeheader()          # ← writes the header row
    writer.writerows(students)
```

- `fieldnames` defines the **column order**
- `writeheader()` writes the header automatically from `fieldnames`

---

## Key Parameters You Should Know

Customize how your CSV is read/written:

```python
csv.reader(file, delimiter=';')      # use semicolons instead of commas
csv.reader(file, delimiter='\t')     # use tabs (TSV files)
```

| Parameter | Default | Purpose |
|---|---|---|
| `delimiter` | `,` | Character separating values |
| `quotechar` | `"` | Character for quoting fields |
| `skipinitialspace` | `False` | Skip space after delimiter |

**Example — reading a semicolon-separated file:**
```python
with open('european_data.csv', 'r', newline='') as f:
    reader = csv.reader(f, delimiter=';')
```

---

## Values That Contain Commas

What happens when a value **itself** contains a comma?

```
name,address,age
Alice,"Naga City, Camarines Sur",18
```

The `csv` module handles this **automatically** using quotes:

```python
import csv
with open('contacts.csv', 'r', newline='') as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
```

**Output:**
```
['name', 'address', 'age']
['Alice', 'Naga City, Camarines Sur', '18']
```

The comma inside quotes is **not** treated as a delimiter. ✅

---

## 🎮 GAME — True or False Buzzer!

**Stand up for TRUE, stay seated for FALSE.**

**Round 1:** `csv` is a third-party module — you must `pip install` it first.

**Round 2:** `csv.DictReader` skips the header row automatically.

**Round 3:** `writerows()` can only write one row at a time.

**Round 4:** `newline=''` in `open()` is recommended when working with CSV files.

**Round 5:** All values read by `csv.reader` are returned as **strings**.

---

## 🎮 ANSWERS — True or False Buzzer!

| Round | Statement | Answer |
|---|---|---|
| 1 | `csv` requires `pip install` | ❌ **FALSE** — it's built-in |
| 2 | `DictReader` skips header automatically | ✅ **TRUE** |
| 3 | `writerows()` writes one row at a time | ❌ **FALSE** — it writes many |
| 4 | `newline=''` is recommended | ✅ **TRUE** |
| 5 | All values from `csv.reader` are strings | ✅ **TRUE** |

---

## Common Errors & How to Fix Them

| Error | Cause | Fix |
|---|---|---|
| `FileNotFoundError` | Wrong file name or path | Check spelling, use full path |
| Extra blank lines in output | Missing `newline=''` | Add `newline=''` to `open()` |
| Values include quote marks | Using wrong `quotechar` | Set `quotechar` parameter |
| Numbers treated as strings | `csv.reader` always returns strings | Convert: `int()`, `float()` |
| Wrong columns in DictWriter | `fieldnames` mismatch | Check your dict keys match `fieldnames` |

---

## 🎮 GAME — Debug the Code!

**This code has 3 bugs. Find them all! (2 minutes)**

```python
import CSV                             # line 1

students = [
    {'name': 'Alice', 'grade': 90},
    {'name': 'Bob',   'grade': 85},
]

with open('out.csv', 'r') as file:    # line 8
    writer = csv.DictWriter(file)     # line 9
    writer.writeheader()
    writer.writerows(students)
```

Hint: Look at the import, the file mode, and the writer setup.

---

## 🎮 ANSWER — Debug the Code!

**Bug 1 — Line 1:** Module name must be **lowercase**
```python
# ❌ Wrong              ✅ Fixed
import CSV        →    import csv
```

**Bug 2 — Line 8:** Wrong file mode — should be `'w'` not `'r'`
```python
# ❌ Wrong              ✅ Fixed
open('out.csv', 'r')  →  open('out.csv', 'w', newline='')
```

**Bug 3 — Line 9:** `DictWriter` requires `fieldnames`
```python
# ❌ Wrong                          ✅ Fixed
csv.DictWriter(file)  →  csv.DictWriter(file, fieldnames=['name','grade'])
```

---

## Real-World Example: Student Gradebook

**Task:** Read a grade file, compute the average, and save a report.

```python
import csv

totals, count = 0, 0

with open('grades.csv', 'r', newline='') as f:
    reader = csv.DictReader(f)
    results = []
    for row in reader:
        score = int(row['score'])
        totals += score
        count += 1
        results.append({'name': row['name'], 'score': score,
                        'passed': 'Yes' if score >= 75 else 'No'})

with open('report.csv', 'w', newline='') as f:
    writer = csv.DictWriter(f, fieldnames=['name', 'score', 'passed'])
    writer.writeheader()
    writer.writerows(results)

print(f"Class average: {totals / count:.2f}")
```

---

## Quick Reference Cheat Sheet

```python
import csv

# READ with reader
with open('file.csv', 'r', newline='') as f:
    for row in csv.reader(f):
        print(row)           # row is a list

# READ with DictReader
with open('file.csv', 'r', newline='') as f:
    for row in csv.DictReader(f):
        print(row['column']) # row is a dict

# WRITE with writer
with open('file.csv', 'w', newline='') as f:
    w = csv.writer(f)
    w.writerow(['col1', 'col2'])
    w.writerows(data)

# WRITE with DictWriter
with open('file.csv', 'w', newline='') as f:
    w = csv.DictWriter(f, fieldnames=['col1','col2'])
    w.writeheader()
    w.writerows(data)
```

---

## Lesson Recap

| Concept | What You Learned |
|---|---|
| CSV format | Plain text, comma-separated, rows & columns |
| `import csv` | Built-in — no install needed |
| `csv.reader` | Reads rows as **lists** |
| `csv.DictReader` | Reads rows as **dicts** (uses header as keys) |
| `csv.writer` | Writes rows from **lists** |
| `csv.DictWriter` | Writes rows from **dicts** |
| `newline=''` | Always include this when opening CSV files |
| `delimiter` | Change separator character if needed |

---

## What's Next?

**You can now work with real-world data! 🎉**

**Practice Challenge:**
> Create a CSV file of your 5 favorite movies (title, year, rating).
> Write a Python script to read the file and print only the movies rated 8 or higher.

**Coming Up Next:**
- 🗄️ Working with **JSON** files
- 🐼 Introduction to **pandas** for data analysis
- 🌐 Fetching CSV data from the **web**

> "Data is the new oil — and CSV is the pipeline."
