# Linked Lists in Python
### Computer Programming 2
### From Objects to Dynamic Data Structures

---

## Slide 1: Motivation

- So far, you've learned:
  - Dictionaries
  - Tuples
  - Sets
  - Classes and Objects
- Now we move into:
  **Data Structures built from objects**

---

## Slide 2: Key Idea Review

- In Python:
  > Everything is an object
- Even:
  - Integers
  - Strings
  - Lists

- You can also create your own objects (classes)

---

## Slide 3: Why This Matters

- Built-in structures (like lists) are powerful
- But:
  - You don’t control how they work internally
- Today:
  👉 You build your own structure

---

## Slide 4: The Problem

- Python lists:
  - Fast indexing ✅
  - Slow insertions (in middle) ❌

- Why?
  - They shift elements in memory

---

## Slide 5: Enter Linked Lists

- A **Linked List** is:
  > A collection of nodes connected by references

- Each element points to the next

---

## Slide 6: Visual Concept

```

[10 | next] → [20 | next] → [30 | None]

````

- Each box = Node
- Each node contains:
  - Data
  - Reference to next node

---

## Slide 7: Node Structure

- A node is just an object:

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
````

---

## Slide 8: Key Insight

* You are using:

  * Classes ✔
  * Objects ✔
* To build:

  * A custom data structure ✔

---

## Slide 9: Interactive Game 1 🧠

**Predict the Output**

```python
a = Node(5)
b = Node(10)

a.next = b

print(a.next.data)
```

Options:
A. 5
B. 10
C. None
D. Error

---

## Slide 10: Answer

✅ Correct Answer: **B. 10**

* `a.next` points to node `b`
* `b.data = 10`

---

## Slide 11: Building a Linked List

```python
head = Node(1)
second = Node(2)
third = Node(3)

head.next = second
second.next = third
```

---

## Slide 12: Traversing the List

```python
current = head

while current:
    print(current.data)
    current = current.next
```

---

## Slide 13: Output

```
1
2
3
```

* Traversal = visiting nodes one by one

---

## Slide 14: Interactive Game 2 🎯

**Fill in the blank**

```python
while current != ______:
    print(current.data)
    current = current.next
```

Options:
A. 0
B. None
C. False
D. ""

---

## Slide 15: Answer

✅ Correct Answer: **B. None**

* End of list is marked by `None`

---

## Slide 16: Inserting at Beginning

```python
new_node = Node(0)
new_node.next = head
head = new_node
```

---

## Slide 17: Inserting at End

```python
current = head

while current.next:
    current = current.next

current.next = Node(4)
```

---

## Slide 18: Interactive Game 3 🧩

What does this do?

```python
current.next = Node(99)
```

A. Deletes node
B. Adds new node after current
C. Replaces current node
D. Ends list

---

## Slide 19: Answer

✅ Correct Answer: **B**

* It links a new node after current

---

## Slide 20: Deleting a Node (Basic)

```python
current = head

while current.next:
    if current.next.data == target:
        current.next = current.next.next
        break
    current = current.next
```

---

## Slide 21: Why Linked Lists?

Advantages:

* Dynamic size
* Fast insertions/deletions
* No shifting needed

---

## Slide 22: Downsides

* No direct indexing
* More memory (extra pointers)
* Slower traversal

---

## Slide 23: Comparison

| Feature       | Python List | Linked List |
| ------------- | ----------- | ----------- |
| Indexing      | Fast        | Slow        |
| Insert/Delete | Slow        | Fast        |
| Memory        | Efficient   | Extra used  |

---

## Slide 24: Interactive Game 4 ⚔️

Which is better?

**Scenario:**
You frequently insert data in the middle.

A. Python List
B. Linked List

---

## Slide 25: Final Answer

✅ Correct Answer: **B. Linked List**

---

## Slide 26: Summary

* Linked Lists are:

  * Built using classes
  * Connected via references
* Core operations:

  * Traverse
  * Insert
  * Delete

---

## Slide 27: Challenge Exercise 🚀

Create a program that:

1. Builds a linked list
2. Adds 5 numbers
3. Prints all values

Bonus:

* Add delete functionality

---

## Slide 28: Closing Thought

> You are no longer just using data structures
> You are now **engineering them**
