# Regular Expressions
## Lesson VI

`re module` · `pattern matching` · `text processing`

*Jerome Loria — March 2026*

---

## 🔍 What is a Regular Expression?

A **regular expression** (regex) is a sequence of characters that defines a *search pattern*.

**📌 Used for**
- Searching text
- Validating input (emails, phone numbers)
- Extracting data
- Replacing/modifying strings

**🌍 Used in**
- Web development
- Data science & scraping
- Cybersecurity
- Log file analysis

> 💡 Think of regex as a **supercharged search bar** that understands patterns, not just exact words.

---

## 🌐 Regex in the Real World

| Use Case | Pattern Description | Example Match |
|---|---|---|
| Email validation | word@word.word | alice@school.edu |
| Phone number | digits with dashes | 09-123-456-7890 |
| ZIP / postal code | 4–5 digits | 1000 (Manila) |
| Password strength | mix of chars + length | P@ssw0rd! |
| Find dates in a log | YYYY-MM-DD | 2024-11-15 |
| HTML tag stripping | \<anything\> | \<b\>Hello\</b\> |

---

## 📦 The `re` Module

Python's built-in module for working with regular expressions.

```python
import re   # Always import first!

# Basic example
text = "Hello, my name is Juan!"
match = re.search(r"Juan", text)

if match:
    print("Found:", match.group())   # Found: Juan
else:
    print("Not found")
```

> 🔑 The **r"..."** prefix creates a *raw string* — backslashes are treated literally. Always use raw strings with regex!

---

## ⚙️ Key Functions in `re`

| Function | What it does | Returns |
|---|---|---|
| `re.search()` | Find first match anywhere | Match object or None |
| `re.match()` | Match at start of string only | Match object or None |
| `re.findall()` | Find all matches | List of strings |
| `re.finditer()` | Find all matches (iterator) | Iterator of Match objects |
| `re.sub()` | Replace matches | New string |
| `re.split()` | Split by pattern | List of strings |
| `re.compile()` | Pre-compile a pattern | Pattern object |

---

## 🔤 Literal Characters

The simplest pattern — match exact characters.

```python
import re

text = "The cat sat on the mat"

result = re.findall(r"at", text)
print(result)  # ['at', 'at', 'at']

result2 = re.findall(r"cat", text)
print(result2)  # ['cat']
```

> Regex is **case-sensitive** by default. `"Cat"` ≠ `"cat"`

---

## ✨ Metacharacters

Characters with special meaning in regex:

| Symbol | Meaning |
|---|---|
| `.` | Any character except newline |
| `^` | Start of string |
| `$` | End of string |
| `\` | Escape special character |
| `\|` | OR operator |
| `( )` | Grouping |

> ⚠️ To match a literal dot, escape it: `\.`

---

## 💡 The Dot `.`

Matches **any single character** except a newline.

```python
import re

words = ["cat", "bat", "hat", "sat", "mat", "rat"]

for w in words:
    if re.match(r".at", w):
        print(w, "✓")

# cat ✓  bat ✓  hat ✓  sat ✓  mat ✓  rat ✓
```

```python
# Match any 3-character word ending in 'at'
text = "cat bat 1at !at"
print(re.findall(r".at", text))  # ['cat', 'bat', '1at', '!at']
```

---

## 🗂️ Character Classes `[ ]`

Match **one character** from a defined set.

```python
import re

text = "cat bat hat sat"

# Match c, b, or h followed by 'at'
print(re.findall(r"[cbh]at", text))   # ['cat', 'bat', 'hat']

# Match any lowercase vowel
print(re.findall(r"[aeiou]", "Hello World"))  # ['e', 'o', 'o']

# Match digits
print(re.findall(r"[0-9]", "a1b2c3"))  # ['1', '2', '3']

# Negation: match anything NOT a digit
print(re.findall(r"[^0-9]", "a1b2c3"))  # ['a', 'b', 'c']
```

---

## ⚡ Shorthand Character Classes

| Shorthand | Equivalent | Matches |
|---|---|---|
| `\d` | `[0-9]` | Any digit |
| `\D` | `[^0-9]` | Any non-digit |
| `\w` | `[a-zA-Z0-9_]` | Word character |
| `\W` | `[^a-zA-Z0-9_]` | Non-word character |
| `\s` | `[ \t\n\r]` | Whitespace |
| `\S` | `[^ \t\n\r]` | Non-whitespace |

```python
text = "My phone is 0917-123-4567"
print(re.findall(r"\d", text))   # ['0','9','1','7','1','2','3','4','5','6','7']
print(re.findall(r"\w+", text))  # ['My', 'phone', 'is', '0917', '123', '4567']
```

---

## ⚓ Anchors: `^` and `$`

Anchors do not match characters — they match **positions**.

```python
import re

# ^ matches START of string
print(re.search(r"^Hello", "Hello World"))  # Match ✓
print(re.search(r"^World", "Hello World"))  # None ✗

# $ matches END of string
print(re.search(r"World$", "Hello World"))  # Match ✓
print(re.search(r"Hello$", "Hello World"))  # None ✗

# Combine both to match EXACT string
print(re.match(r"^\d{4}$", "2024"))   # Match ✓  (exactly 4 digits)
print(re.match(r"^\d{4}$", "20245"))  # None ✗   (5 digits)
```

---

## 🔢 Quantifiers

Specify **how many times** a pattern should repeat.

| Quantifier | Meaning | Example |
|---|---|---|
| `*` | 0 or more | `ab*c` → ac, abc, abbc… |
| `+` | 1 or more | `ab+c` → abc, abbc… (not ac) |
| `?` | 0 or 1 (optional) | `colou?r` → color, colour |
| `{n}` | Exactly n times | `\d{4}` → 2024 |
| `{n,}` | n or more | `\d{3,}` → 123, 1234… |
| `{n,m}` | Between n and m | `\d{2,4}` → 12, 123, 1234 |

---

## 🧪 Quantifiers in Action

```python
import re

# + : one or more digits
print(re.findall(r"\d+", "abc123def456"))  # ['123', '456']

# * : zero or more 'a's before 'b'
print(re.findall(r"a*b", "b ab aab aaab"))  # ['b', 'ab', 'aab', 'aaab']

# ? : optional 'u'
words = ["color", "colour", "colr"]
for w in words:
    if re.match(r"colou?r", w):
        print(w, "matches!")  # color, colour match

# {2,4}: 2 to 4 word characters
print(re.findall(r"\w{2,4}", "I am a student"))  # ['am', 'stud', 'ent']
```

---

## 🗃️ Groups `( )`

Parentheses **group** parts of a pattern and **capture** matched text.

```python
import re

# Extract year, month, day separately
date = "Today is 2024-11-15"
match = re.search(r"(\d{4})-(\d{2})-(\d{2})", date)

if match:
    print("Full match:", match.group(0))   # 2024-11-15
    print("Year:",       match.group(1))   # 2024
    print("Month:",      match.group(2))   # 11
    print("Day:",        match.group(3))   # 15

# Named groups — even cleaner!
match2 = re.search(r"(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})", date)
print(match2.group("year"))   # 2024
print(match2.group("month"))  # 11
```

---

## 🔀 Alternation `|`

The pipe `|` acts as an **OR** operator between patterns.

```python
import re

# Match 'cat' or 'dog'
animals = ["I have a cat", "She has a dog", "He has a bird"]
for sentence in animals:
    if re.search(r"cat|dog", sentence):
        print(sentence, "→ has a pet!")

# Match multiple file extensions
files = ["report.pdf", "photo.jpg", "data.csv", "notes.txt", "hack.exe"]
for f in files:
    if re.search(r"\.(pdf|jpg|csv|txt)$", f):
        print(f, "→ safe file ✓")
```

> Use groups to limit the scope of `|`: `gr(a|e)y` matches "gray" and "grey".

---

## 🔎 `search()` vs `match()`

```python
text = "  Python is awesome"

# match() only checks the BEGINNING
m1 = re.match(r"Python", text)
print(m1)   # None — text starts with spaces!

# search() scans the WHOLE string
m2 = re.search(r"Python", text)
print(m2)   # <re.Match object; match='Python'>

# match() works if pattern is at start
m3 = re.match(r"\s+Python", text)
print(m3.group())   # '  Python' ✓
```

**match()** — Use when input must start with pattern (e.g., validating format from pos 0)

**search()** — Use when pattern can appear anywhere in the string

---

## 📋 `findall()` & `finditer()`

```python
import re

text = "Scores: 85, 90, 78, 92, 88"

# findall → returns a plain list
scores = re.findall(r"\d+", text)
print(scores)  # ['85', '90', '78', '92', '88']
print([int(s) for s in scores])  # convert to ints

# finditer → returns iterator with position info
for m in re.finditer(r"\d+", text):
    print(f"Found '{m.group()}' at position {m.start()}–{m.end()}")
```

```
Found '85' at position 8–10
Found '90' at position 12–14
Found '78' at position 16–18
```

> Use `finditer()` when you need position info; use `findall()` when you just need the values.

---

## ✏️ `re.sub()` — Find & Replace

```python
import re

# Syntax: re.sub(pattern, replacement, string, count=0)

# Censor bad words
text = "This is spam and more spam!"
clean = re.sub(r"spam", "****", text)
print(clean)  # This is **** and more ****!

# Remove all digits
msg = "My PIN is 1234 and OTP is 5678"
print(re.sub(r"\d", "*", msg))  # My PIN is **** and OTP is ****

# Format phone numbers: 09171234567 → 0917-123-4567
phone = "Call 09171234567 now"
formatted = re.sub(r"(0\d{3})(\d{3})(\d{4})", r"\1-\2-\3", phone)
print(formatted)  # Call 0917-123-4567 now
```

---

## ✂️ `re.split()` — Split by Pattern

```python
# Split by any whitespace (spaces, tabs, newlines)
text = "one   two\tthree\nfour"
print(re.split(r"\s+", text))
# ['one', 'two', 'three', 'four']

# Split by comma, semicolon, or pipe
csv = "Alice,30;Bob|25,Carol;22"
print(re.split(r"[,;|]", csv))
# ['Alice', '30', 'Bob', '25', 'Carol', '22']

# Split a sentence into words and punctuation
sentence = "Hello, world! How are you?"
tokens = re.split(r"(\W+)", sentence)
print(tokens)
```

> Wrapping the delimiter in a group `( )` includes it in the output list — useful for tokenization!

---

## ⚙️ `re.compile()` — Reusable Patterns

When you use the same pattern many times, **compile** it for efficiency.

```python
import re

# Without compile — pattern is re-parsed every call
for line in open("students.txt"):
    re.search(r"\d{5}", line)   # slightly slower

# With compile — parsed ONCE, reused many times
id_pattern = re.compile(r"\d{5}")

lines = ["ID: 12345 — Alice", "ID: 67890 — Bob", "No ID here"]
for line in lines:
    m = id_pattern.search(line)
    if m:
        print("Found ID:", m.group())

# Compiled patterns have the same methods
ids = id_pattern.findall("12345 and 67890 and 99999")
```

---

## 🚩 Regex Flags

Flags modify how the pattern is applied.

| Flag | Short | Effect |
|---|---|---|
| `re.IGNORECASE` | `re.I` | Case-insensitive matching |
| `re.MULTILINE` | `re.M` | `^`/`$` match each line |
| `re.DOTALL` | `re.S` | `.` matches newline too |
| `re.VERBOSE` | `re.X` | Allow comments & whitespace |

```python
text = "Hello WORLD hello world"

# IGNORECASE
print(re.findall(r"hello", text, re.I))  # ['Hello', 'hello']

# MULTILINE
lines = "start\nstop\nstart again"
print(re.findall(r"^start", lines, re.M))  # ['start', 'start']
```

---

## 📝 The `re.VERBOSE` Flag

Makes complex patterns **readable** with comments and whitespace.

```python
import re

# Hard to read
phone_re = re.compile(r"(\+63|0)(\d{3})-?(\d{3})-?(\d{4})")

# Same pattern with re.VERBOSE — much cleaner!
phone_re = re.compile(r"""
    (\+63|0)    # country code or leading 0
    (\d{3})     # area/network code (e.g., 917)
    -?          # optional dash
    (\d{3})     # first 3 digits
    -?          # optional dash
    (\d{4})     # last 4 digits
""", re.VERBOSE)

matches = phone_re.findall("Call +63917-123-4567 or 0917-987-6543")
print(matches)
```

---

## 📧 Practice: Email Validation

```python
import re

def is_valid_email(email):
    pattern = re.compile(r"""
        ^                   # start
        [a-zA-Z0-9._%+-]+   # username (letters, digits, dots, etc.)
        @                   # literal @
        [a-zA-Z0-9.-]+      # domain name
        \.                  # literal dot
        [a-zA-Z]{2,}        # top-level domain (2+ letters)
        $                   # end
    """, re.VERBOSE)
    return bool(pattern.match(email))

emails = [
    "student@school.edu",    # ✓
    "invalid-email",         # ✗
    "user@.com",             # ✗
    "first.last@uni.edu.ph"  # ✓
]
for e in emails:
    status = "✓ Valid" if is_valid_email(e) else "✗ Invalid"
    print(f"{e:<30} {status}")
```

---

## 🔐 Practice: Password Strength Checker

```python
import re

def check_password(password):
    errors = []
    if not re.search(r".{8,}", password):
        errors.append("At least 8 characters")
    if not re.search(r"[A-Z]", password):
        errors.append("At least one uppercase letter")
    if not re.search(r"[a-z]", password):
        errors.append("At least one lowercase letter")
    if not re.search(r"\d", password):
        errors.append("At least one digit")
    if not re.search(r"[!@#$%^&*]", password):
        errors.append("At least one special character")
    if errors:
        print("❌ Weak password. Missing:")
        for e in errors: print("  •", e)
    else:
        print("✅ Strong password!")

check_password("hello")      # ❌ many issues
check_password("P@ssw0rd!")  # ✅
```

---

## 📋 Practice: Parsing Log Files

```python
import re

log = """
2024-11-15 08:32:11 INFO  User 'alice' logged in from 192.168.1.10
2024-11-15 08:45:03 ERROR Failed login attempt from 10.0.0.5
2024-11-15 09:01:55 INFO  User 'bob' logged in from 172.16.0.2
2024-11-15 09:15:22 WARN  High memory usage detected
"""

# Extract timestamps
timestamps = re.findall(r"\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}", log)
print("Timestamps:", timestamps)

# Find all IP addresses
ips = re.findall(r"\b\d{1,3}(?:\.\d{1,3}){3}\b", log)
print("IPs found:", ips)

# Find ERROR and WARN lines
issues = re.findall(r"(ERROR|WARN).+", log)
for i in issues: print(" ", i)
```

---

## 👀 Lookahead & Lookbehind

| Syntax | Type | Meaning |
|---|---|---|
| `(?=...)` | Lookahead | Match if followed by … |
| `(?!...)` | Negative lookahead | Match if NOT followed by … |
| `(?<=...)` | Lookbehind | Match if preceded by … |
| `(?<!...)` | Negative lookbehind | Match if NOT preceded by … |

```python
prices = "apple: 50, banana: 30, mango: 75"

# Get numbers preceded by ': '
nums = re.findall(r"(?<=: )\d+", prices)
print(nums)   # ['50', '30', '75']

# Get words followed by ':'
items = re.findall(r"\w+(?=:)", prices)
print(items)  # ['apple', 'banana', 'mango']
```

---

## ⚠️ Common Mistakes to Avoid

**❌ Forgetting raw strings**
```python
re.findall("\d+", text)   # Wrong — \d is an escape seq
re.findall(r"\d+", text)  # Right
```

**❌ Unescaped dots**
```python
re.match(r"google.com", url)   # Matches "google_com" too!
re.match(r"google\.com", url)  # Escape the dot
```

**❌ match() vs search()**
```python
re.match(r"\d+", "abc 123")   # Won't work if not at start
re.search(r"\d+", "abc 123")  # Use search() instead
```

---

## 📌 Regex Quick Reference

**Characters** · **Anchors** · **Quantifiers** · **Functions**

| | | | |
|---|---|---|---|
| `.` | Any char (not \n) | `^` | Start of string/line |
| `\d \D` | Digit / Non-digit | `$` | End of string/line |
| `\w \W` | Word / Non-word | `\b` | Word boundary |
| `\s \S` | Space / Non-space | `*` | 0 or more |
| `[abc]` | One of a, b, c | `+` | 1 or more |
| `[^abc]` | Not a, b, or c | `?` | 0 or 1 |
| `search()` | First match anywhere | `{n,m}` | n to m |
| `findall()` | All matches (list) | `*? +? ??` | Lazy versions |
| `sub()` | Replace matches | `split()` | Split by pattern |

---

## 🎓 Recommended Next Steps

- **regex101.com** — Live regex tester
- **regexlearn.com** — Interactive exercises
- **Python docs** — docs.python.org/3/library/re
- Build: email validator, form checker, log parser
- Explore: `re.VERBOSE` for readable patterns