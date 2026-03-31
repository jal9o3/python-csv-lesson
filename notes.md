# Trees & Recursion in Python

> *"To understand recursion, you must first understand recursion."*

---

## What We'll Cover Today

1. 🔁 What is Recursion?
2. 🌳 What is a Tree?
3. 🧩 Tree Terminology
4. 💻 Implementing Trees in Python
5. 🔍 Tree Traversals
6. 🎮 Interactive Challenges

---

## Part 1: Recursion

### A Function That Calls Itself

Recursion is when a function **calls itself** to solve a smaller version of the same problem.

```python
def countdown(n):
    if n == 0:
        print("Blast off! 🚀")
        return
    print(n)
    countdown(n - 1)  # calls itself!

countdown(5)
```

**Output:** `5 4 3 2 1 Blast off! 🚀`

---

## The Two Rules of Recursion

Every recursive function **must** have:

### 1. 🛑 A Base Case
The condition where the function **stops** calling itself.

### 2. 🔄 A Recursive Case
The part where the function **calls itself** with a simpler input.

> Without a base case → **Infinite loop → Stack Overflow 💥**

---

## 🎮 Game: Spot the Base Case!

### Look at this code. What is the base case?

```python
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)
```

**Think for 30 seconds, then discuss with your neighbor!**

- A) `return n * factorial(n - 1)`
- B) `if n == 0: return 1`  ✅
- C) `def factorial(n):`
- D) There is no base case

*Follow-up: What happens if you call `factorial(-1)`?*

---

## Visualizing the Call Stack

### `factorial(4)` step by step:

```
factorial(4)
  → 4 * factorial(3)
        → 3 * factorial(2)
              → 2 * factorial(1)
                    → 1 * factorial(0)
                          → returns 1
                    → returns 1 * 1 = 1
              → returns 2 * 1 = 2
        → returns 3 * 2 = 6
  → returns 4 * 6 = 24
```

Each call **waits** for the one below it. This is the **call stack**.

---

## Recursion vs. Iteration

| | Recursion | Iteration |
|---|---|---|
| **Uses** | Function calls | Loops |
| **Base case** | Required | Loop condition |
| **Memory** | Call stack | Loop variables |
| **Readability** | Often cleaner | More explicit |
| **Risk** | Stack overflow | Infinite loop |

```python
# Iterative                  # Recursive
def sum_iter(n):              def sum_rec(n):
    total = 0                     if n == 0:
    for i in range(n+1):              return 0
        total += i                return n + sum_rec(n-1)
    return total
```

---

## 🎮 Game: Trace the Recursion

### What does this print? Work it out on paper!

```python
def mystery(n):
    if n <= 0:
        return
    mystery(n - 2)
    print(n)

mystery(6)
```

**Hint:** The print happens *after* the recursive call!

*Answer:* `2  4  6`  
*(prints on the way **back up** the call stack)*

---

## Part 2: Trees 🌳

### Trees Are Everywhere

- 📁 **File systems** — folders inside folders
- 🌐 **HTML/DOM** — tags nested inside tags
- 🏢 **Org charts** — CEO → Managers → Employees
- 🔤 **Decision trees** — branching yes/no questions
- 💾 **Databases** — B-trees for fast searching

> A **tree** is a hierarchical data structure made of **nodes** connected by **edges**.

---

## Tree Terminology 🧩

```
          [A]          ← Root
         /   \
       [B]   [C]       ← Children of A
      /   \     \
    [D]   [E]   [F]    ← Leaves
```

| Term | Meaning |
|---|---|
| **Root** | Top node (no parent) |
| **Node** | Each element in the tree |
| **Edge** | Connection between nodes |
| **Leaf** | Node with no children |
| **Parent / Child** | Relative nodes |
| **Height** | Longest root-to-leaf path |
| **Depth** | Distance from root |

---

## 🎮 Game: Count the Tree!

### Given this tree, answer the questions:

```
            [10]
           /    \
         [5]    [20]
        /   \      \
      [3]   [7]    [25]
                  /
               [22]
```

In your notebook, write down:
1. **How many nodes** are there? *(Answer: 7)*
2. **What is the height** of the tree? *(Answer: 3)*
3. **What are the leaves?** *(Answer: 3, 7, 22)*
4. **What is the depth of node 22?** *(Answer: 3)*

---

## Binary Trees

A **Binary Tree** is a tree where each node has **at most 2 children**: a **left** and a **right** child.

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
```

### Building a tree manually:

```python
root = Node(10)
root.left = Node(5)
root.right = Node(20)
root.left.left = Node(3)
root.left.right = Node(7)
```

---

## The `TreeNode` Class in Full

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None    # left child
        self.right = None   # right child

    def __repr__(self):
        return f"TreeNode({self.value})"


# Create this tree:
#       1
#      / \
#     2   3

root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)

print(root)        # TreeNode(1)
print(root.left)   # TreeNode(2)
```

---

## Part 3: Tree Traversals 🔍

### How do we *visit* every node?

There are **three classic** ways to traverse a binary tree:

| Traversal | Order | Mnemonic |
|---|---|---|
| **Pre-order** | Root → Left → Right | **P**arents first |
| **In-order** | Left → Root → Right | **I**n-between |
| **Post-order** | Left → Right → Root | **P**arents last |

> All three use **recursion** naturally! 🎉

---

## Pre-order Traversal

### Visit Root → Left → Right

```python
def preorder(node):
    if node is None:     # base case!
        return
    print(node.value)    # visit root first
    preorder(node.left)  # then left subtree
    preorder(node.right) # then right subtree
```

```
       [A]
      /   \
    [B]   [C]
   /   \
 [D]   [E]
```

**Pre-order result:** `A  B  D  E  C`

*Use case: Copying a tree, serializing structure*

---

## In-order Traversal

### Visit Left → Root → Right

```python
def inorder(node):
    if node is None:     # base case!
        return
    inorder(node.left)   # left subtree first
    print(node.value)    # then root
    inorder(node.right)  # then right subtree
```

```
       [5]
      /   \
    [3]   [7]
```

**In-order result:** `3  5  7`

> 💡 **Key insight:** In-order traversal of a **Binary Search Tree**
> always gives values in **sorted order**!

---

## Post-order Traversal

### Visit Left → Right → Root

```python
def postorder(node):
    if node is None:     # base case!
        return
    postorder(node.left)   # left subtree first
    postorder(node.right)  # right subtree
    print(node.value)      # root LAST
```

```
       [A]
      /   \
    [B]   [C]
```

**Post-order result:** `B  C  A`

*Use case: Deleting a tree (delete children before parent),
evaluating expression trees*

---

## 🎮 Game: Traversal Race!

### Trace all three traversals on this tree:

```
           [1]
          /   \
        [2]   [3]
       /   \
     [4]   [5]
```

Race your neighbor! First to correctly write all three wins 🏆

| Traversal | Your Answer |
|---|---|
| Pre-order | `1  2  4  5  3` |
| In-order | `4  2  5  1  3` |
| Post-order | `4  5  2  3  1` |

*Check: pre-order always starts with the root!*

---

## 🎮 Game: Build-a-Tree!

### Given this pre-order and in-order output, draw the tree!

```
Pre-order:  [A, B, D, E, C, F]
In-order:   [D, B, E, A, F, C]
```

**Rules:**
- Pre-order → first element is always the **root**
- In-order → root splits the list into **left** and **right** subtrees
- Work in groups of 3. Draw the tree on paper.

*(Answer)*
```
         [A]
        /   \
      [B]   [C]
     /   \ /
   [D] [E][F]
```

---

## Recursive Tree Height

### How tall is a tree? Recursion makes this elegant!

```python
def height(node):
    # Base case: empty tree has height -1 (or 0 by convention)
    if node is None:
        return -1

    # Recursive case: height = 1 + max(left height, right height)
    left_height = height(node.left)
    right_height = height(node.right)

    return 1 + max(left_height, right_height)

# Test it:
root = TreeNode(10)
root.left = TreeNode(5)
root.left.left = TreeNode(3)
print(height(root))  # Output: 2
```

---

## Counting Nodes Recursively

```python
def count_nodes(node):
    if node is None:       # base case
        return 0
    return (1 +
            count_nodes(node.left) +   # count left
            count_nodes(node.right))   # count right

# Searching for a value:
def search(node, target):
    if node is None:           # not found
        return False
    if node.value == target:   # found!
        return True
    return (search(node.left, target) or
            search(node.right, target))
```

> **Notice the pattern:** base case + recursive case on left + recursive case on right

---

## Binary Search Trees (BST)

A **BST** is a binary tree with one special rule:

> For any node:
> - All values in the **left** subtree are **smaller**
> - All values in the **right** subtree are **larger**

```
         [8]
        /   \
      [3]   [10]
     /   \     \
   [1]   [6]   [14]
        /   \
      [4]   [7]
```

**Search is fast!** At each node, go left or right — no need to check both.
Time complexity: **O(h)** where h = height of tree.

---

## BST Insert

```python
def insert(node, value):
    # Base case: found an empty spot
    if node is None:
        return TreeNode(value)

    # Recursive case: go left or right
    if value < node.value:
        node.left = insert(node.left, value)
    elif value > node.value:
        node.right = insert(node.right, value)
    # If equal, we ignore (no duplicates)

    return node

# Usage:
root = None
for val in [8, 3, 10, 1, 6]:
    root = insert(root, val)
```

---

## 🎮 Final Boss: Debug the Tree! 🐛

### This code has **3 bugs**. Find them all!

```python
def inorder(node):
    if node = None:          # Bug 1
        return
    inorder(node.right)      # Bug 2
    print(node.value)
    inorder(node.left)       # Bug 2 continued

def height(node):
    if node is None:
        return 0
    return max(height(node.left),   # Bug 3
               height(node.right))
```

**Discuss with your table. You have 3 minutes!**

*Bugs: (1) `=` should be `==`  (2) right/left are swapped  (3) missing `+ 1`*

---

## Summary: The Big Picture 🗺️

| Concept | Key Idea |
|---|---|
| **Recursion** | Function calls itself; needs base + recursive case |
| **Call Stack** | Each call waits for the next to finish |
| **Tree** | Hierarchical structure of nodes |
| **Binary Tree** | At most 2 children per node |
| **Pre-order** | Root → Left → Right |
| **In-order** | Left → Root → Right (sorted for BST!) |
| **Post-order** | Left → Right → Root |
| **BST** | Left < Root < Right at every node |

---

## What's Next? 🚀

### Topics that build on today:

- 🔴 **Red-Black Trees** — Self-balancing BSTs
- 🗄️ **Heaps** — Priority queues, heap sort
- 🔗 **Graphs** — Trees without the "no cycles" rule
- 🌐 **Tries** — Trees for strings/autocomplete
- 🧠 **Decision Trees** — Machine learning fundamentals

### Practice Problems:
1. Write a function that finds the **maximum value** in a BST
2. Write a function that checks if a binary tree is **symmetric**
3. Given two trees, check if they are **identical**

---

## Thank You! 🌳