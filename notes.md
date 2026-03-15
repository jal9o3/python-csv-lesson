# Classes and Objects in Python
### Understanding Object-Oriented Programming
Python Programming Lesson

---

## Lesson Objectives

By the end of this lesson you will be able to:

- Explain what a **class** and **object** are
- Create classes in Python
- Use **attributes** and **methods**
- Instantiate objects
- Apply classes to solve problems

---

## What is Object-Oriented Programming?

Object-Oriented Programming (OOP) is a programming paradigm based on **objects**.

Objects contain:

- **Data (attributes)**
- **Behavior (methods)**

OOP helps organize code into reusable structures.

---

## Real World Analogy

Think about a **Car** 🚗

Car characteristics:
- Color
- Brand
- Speed

Car behaviors:
- Start
- Stop
- Accelerate

In programming:

- **Class = Blueprint**
- **Object = Actual Car**

---

## What is a Class?

A **class** is a blueprint used to create objects.

Example idea:

Class: `Dog`

Possible attributes:
- name
- age

Possible behaviors:
- bark
- sleep

---

## Python Class Syntax

```python
class Dog:
    pass
````

Explanation:

* `class` keyword defines a class
* `Dog` is the class name
* `pass` means empty for now

---

## Creating an Object

```python
class Dog:
    pass

my_dog = Dog()
```

`my_dog` is now an **object** created from the class `Dog`.

---

## Attributes

Attributes store **data about an object**.

Example:

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

---

## What is `__init__`?

`__init__` is a **constructor method**.

It runs automatically when a new object is created.

Example:

```python
dog1 = Dog("Buddy", 3)
```

Now:

* `dog1.name` → Buddy
* `dog1.age` → 3

---

## Accessing Attributes

Example:

```python
print(dog1.name)
print(dog1.age)
```

Output:

```
Buddy
3
```

---

## Methods

Methods define **behavior** of objects.

Example:

```python
class Dog:

    def __init__(self, name):
        self.name = name

    def bark(self):
        print(self.name + " says Woof!")
```

---

## Using Methods

```python
dog1 = Dog("Buddy")

dog1.bark()
```

Output:

```
Buddy says Woof!
```

---

# GAME 1 🎮

## Predict the Output

```python
class Cat:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(self.name + " says Meow")

cat1 = Cat("Luna")
cat1.speak()
```

Question:

What will the output be?

A. Luna says Woof
B. Luna says Meow
C. Error

---

# GAME 2 🎮

## Spot the Error

```python
class Student
    def __init__(self, name):
        self.name = name
```

Find the mistake.

Hint:
Look carefully at the **class declaration line**.

---

# GAME 3 🎮

## Build the Class

Create a class called **Book**.

Attributes:

* title
* author

Objects should be created like this:

```python
b1 = Book("Harry Potter", "J.K. Rowling")
```

Challenge:

Write the `__init__` method.

---

## Example Solution

```python
class Book:

    def __init__(self, title, author):
        self.title = title
        self.author = author
```

---

## Multiple Objects

One class can create **many objects**.

Example:

```python
dog1 = Dog("Buddy")
dog2 = Dog("Max")
dog3 = Dog("Bella")
```

Each object has its **own data**.

---

## Object State

Example:

```
dog1.name = Buddy
dog2.name = Max
dog3.name = Bella
```

All come from the **same class blueprint**.

---

## Adding More Methods

```python
class Dog:

    def __init__(self, name):
        self.name = name

    def bark(self):
        print("Woof!")

    def greet(self):
        print("Hello, I am " + self.name)
```

---

# GAME 4 🎮

## Class Design Challenge

Design a class called **Player**

Attributes:

* name
* score

Methods:

* `add_score(points)`
* `show_score()`

Think about how the code would look.

---

## Possible Implementation

```python
class Player:

    def __init__(self, name):
        self.name = name
        self.score = 0

    def add_score(self, points):
        self.score += points

    def show_score(self):
        print(self.name, self.score)
```

---

# GAME 5 🎮

## True or False

1️⃣ A class is an object.
2️⃣ An object is created from a class.
3️⃣ `__init__` runs when an object is created.

Discuss with a partner.

---

## Quick Practice

Create a class called **Car** with:

Attributes:

* brand
* speed

Method:

* `drive()` → prints `"The car is driving"`.

---

## Why Use Classes?

Classes help:

* Organize code
* Reuse structures
* Model real-world systems
* Manage large programs

---

## Summary

Today you learned:

* Classes
* Objects
* Attributes
* Methods
* Constructors (`__init__`)
* Creating multiple objects

---

## Final Challenge

Create a class called **BankAccount**

Attributes:

* owner
* balance

Methods:

* deposit(amount)
* withdraw(amount)
* display_balance()

Bonus: Prevent withdrawing more than the balance.

---

# Thank You!

Questions?
