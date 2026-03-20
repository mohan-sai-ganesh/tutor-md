# The Complete Data Structures & Algorithms Tutorial
### Every Concept Explained Clearly — From Zero to Interview-Ready (Python)

---

## Table of Contents

### Part 1: Foundations — How to Think About DSA
1. What are Data Structures and Algorithms?
2. Why DSA Matters
3. How to Approach Problem Solving
4. Big O Notation — Measuring Efficiency
5. Time Complexity — How Fast?
6. Space Complexity — How Much Memory?
7. Recursion — Functions That Call Themselves
8. Recursion Patterns and Practice

### Part 2: Linear Data Structures
9. Arrays and Lists
10. Strings
11. Linked Lists
12. Stacks
13. Queues
14. Hash Maps (Dictionaries)
15. Sets

### Part 3: Non-Linear Data Structures
16. Trees — Concepts and Terminology
17. Binary Trees
18. Binary Search Trees (BST)
19. AVL Trees (Self-Balancing BSTs)
20. Heaps and Priority Queues
21. Tries (Prefix Trees)
22. Graphs — Concepts and Terminology
23. Graph Representations
24. Union-Find (Disjoint Set Union)

### Part 4: Core Algorithms
25. Searching Algorithms
26. Sorting Algorithms
27. Two Pointers Technique
28. Sliding Window Technique
29. Binary Search — Beyond Arrays
30. BFS (Breadth-First Search)
31. DFS (Depth-First Search)
32. Topological Sort
33. Shortest Path Algorithms (Dijkstra, Bellman-Ford)
34. Minimum Spanning Trees (Kruskal, Prim)

### Part 5: Advanced Algorithmic Techniques
35. Greedy Algorithms
36. Dynamic Programming — Concepts
37. Dynamic Programming — 1D Problems
38. Dynamic Programming — 2D Problems
39. Dynamic Programming — Classic Problems
40. Backtracking
41. Divide and Conquer
42. Bit Manipulation

### Part 6: Advanced Data Structures
43. Segment Trees
44. Binary Indexed Trees (Fenwick Trees)
45. Monotonic Stack and Monotonic Queue
46. LRU Cache
47. Bloom Filters

### Part 7: Problem-Solving Patterns
48. Pattern: Frequency Counter
49. Pattern: Fast and Slow Pointers
50. Pattern: Merge Intervals
51. Pattern: Cyclic Sort
52. Pattern: Top-K Elements
53. Pattern: Modified Binary Search
54. Pattern: Subsets / Combinations / Permutations
55. Pattern: Matrix Traversal
56. Pattern: Knapsack Variations

### Part 8: Interview Preparation
57. How to Solve Problems in Interviews
58. The Most Important 50 Problems (by Category)
59. Common Mistakes and How to Avoid Them
60. Where to Go Next

---

# PART 1: FOUNDATIONS — HOW TO THINK ABOUT DSA

---

## 1. What are Data Structures and Algorithms?

### 1.1 Data Structures — Ways to Organize Data

A **data structure** is a way of organizing and storing data so that it can be used efficiently. Different data structures are good at different things.

Think of data structures like different containers:

- A **bookshelf** (array): Books are in order, and you can quickly find the 5th book, but inserting a book in the middle means moving everything after it.
- A **chain of paper clips** (linked list): Easy to add or remove clips anywhere, but to find the 50th clip, you have to follow the chain from the beginning.
- A **dictionary** (hash map): You look up a word instantly by name, no need to flip through pages in order.
- A **family tree** (tree): Data arranged in a hierarchy — parent-child relationships.
- A **road map** (graph): Cities connected by roads, where you need to find the shortest route.

### 1.2 Algorithms — Step-by-Step Instructions

An **algorithm** is a step-by-step procedure for solving a problem. Like a recipe:

```
Recipe to find the largest number in a list:
1. Assume the first number is the largest
2. Look at each remaining number
3. If it's bigger than the current largest, it becomes the new largest
4. After checking all numbers, you have the largest
```

```python
def find_largest(numbers):
    largest = numbers[0]
    for num in numbers:
        if num > largest:
            largest = num
    return largest
```

### 1.3 The Relationship

Data structures and algorithms work together. The choice of data structure affects which algorithms you can use efficiently, and the problem you're solving determines which data structure is best.

Looking up a name in an unsorted list → scan everything (slow).
Looking up a name in a hash map → instant lookup (fast).
Same operation, different data structure, vastly different speed.

---

## 2. Why DSA Matters

### 2.1 For Software Engineering

The difference between a good solution and a bad solution is often the difference between a program that runs in 1 second and one that runs in 1 hour. Real-world example:

- **Bad:** Searching for a user in a list of 1 million users by checking each one → up to 1 million comparisons
- **Good:** Searching in a hash map → 1 comparison (average)
- **Bad:** Finding the shortest route by checking all possible paths → billions of years for 20 cities
- **Good:** Using Dijkstra's algorithm → milliseconds

### 2.2 For Interviews

Almost every top tech company (Google, Amazon, Meta, Apple, Microsoft) tests DSA in coding interviews. It's the universal language of technical interviews.

### 2.3 For Problem Solving

DSA teaches you to **think systematically** about problems. Even when you're not using a specific algorithm, the thinking patterns transfer to every area of software development.

---

## 3. How to Approach Problem Solving

### 3.1 The UMPIRE Method

**U — Understand** the problem
- Read it carefully. Twice.
- Identify the inputs and outputs.
- What are the constraints? (size of input, range of values)
- Walk through examples by hand.

**M — Match** with known patterns
- Does this look like a problem I've seen before?
- Which data structure or algorithm category does it fit?

**P — Plan** your approach
- Write out the steps in plain English before coding.
- What data structure will you use?
- What's the algorithm?

**I — Implement** the solution
- Translate your plan into code.
- Write clean, readable code.

**R — Review** your solution
- Trace through your code with the examples.
- Check edge cases (empty input, single element, duplicates, negative numbers).

**E — Evaluate** complexity
- What's the time complexity?
- What's the space complexity?
- Can you do better?

---

## 4. Big O Notation — Measuring Efficiency

### 4.1 What is Big O?

Big O notation describes how the performance of an algorithm changes as the input size grows. It tells you the **worst-case** scaling behavior.

It doesn't tell you exactly how many milliseconds an algorithm takes — it tells you the **growth rate**. Does it get 2x slower when data doubles? Or 4x slower? Or 1000x slower?

### 4.2 The Common Big O Classes

```
O(1)        Constant     → Always the same speed, no matter the input size
O(log n)    Logarithmic  → Grows very slowly. Doubling input adds one step.
O(n)        Linear       → Grows proportionally. Double input = double time.
O(n log n)  Linearithmic → Slightly more than linear. Efficient sorting.
O(n²)       Quadratic    → Double input = 4x time. Nested loops.
O(n³)       Cubic        → Double input = 8x time. Triple nested loops.
O(2ⁿ)       Exponential  → Each new element doubles the time. Very slow.
O(n!)       Factorial    → Grows insanely fast. Unusable for large inputs.
```

### 4.3 How They Compare (n = input size)

```
n        O(1)  O(log n)  O(n)    O(n log n)  O(n²)       O(2ⁿ)
10       1     3         10      33          100         1,024
100      1     7         100     664         10,000      1.27 × 10³⁰
1,000    1     10        1,000   9,966       1,000,000   TOO BIG
10,000   1     13        10,000  132,877     100,000,000 TOO BIG
```

As you can see, O(n²) is already 100 million operations for n=10,000. O(2ⁿ) is essentially infinite for anything above n=30.

### 4.4 How to Calculate Big O

**Rule 1: Drop constants.** O(2n) → O(n). O(500) → O(1).

**Rule 2: Drop lower-order terms.** O(n² + n) → O(n²). O(n + log n) → O(n).

**Rule 3: Different inputs = different variables.** If a function takes two arrays of size a and b, processing both is O(a + b), not O(n).

### 4.5 Examples

```python
# O(1) — Constant
def get_first(arr):
    return arr[0]   # Always one operation, regardless of array size

# O(n) — Linear
def find_sum(arr):
    total = 0
    for num in arr:        # Loops through every element once
        total += num
    return total

# O(n²) — Quadratic
def find_duplicates(arr):
    for i in range(len(arr)):          # Loop 1: n times
        for j in range(i+1, len(arr)): # Loop 2: up to n times
            if arr[i] == arr[j]:       # Total: roughly n × n = n²
                return True
    return False

# O(log n) — Logarithmic (binary search)
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1       # Eliminate half the remaining elements
        else:
            right = mid - 1      # Each step cuts the problem in half
    return -1
```

---

## 5. Time Complexity — How Fast?

### 5.1 Best, Average, and Worst Case

```
Linear search for a value in an array of n elements:
  Best case:    O(1)   — the value is the first element
  Average case: O(n/2) = O(n) — it's somewhere in the middle
  Worst case:   O(n)   — it's the last element or not there at all
```

Big O usually refers to the **worst case** unless stated otherwise.

### 5.2 Common Operations Cheat Sheet

```
Data Structure       Access    Search    Insert    Delete
Array/List           O(1)      O(n)      O(n)*     O(n)*
Linked List          O(n)      O(n)      O(1)**    O(1)**
Hash Map/Set         —         O(1)†     O(1)†     O(1)†
Stack (push/pop)     —         —         O(1)      O(1)
Queue (enq/deq)      —         —         O(1)      O(1)
Binary Search Tree   —         O(log n)  O(log n)  O(log n)
Heap (min/max)       —         O(n)      O(log n)  O(log n)

* Array insert/delete at end is O(1), in middle is O(n)
** Linked list insert/delete given a reference to the node
† Average case — worst case is O(n) for hash collisions
```

### 5.3 Sorting Algorithms Complexity

```
Algorithm          Best        Average     Worst       Space
Bubble Sort        O(n)        O(n²)       O(n²)       O(1)
Selection Sort     O(n²)       O(n²)       O(n²)       O(1)
Insertion Sort     O(n)        O(n²)       O(n²)       O(1)
Merge Sort         O(n log n)  O(n log n)  O(n log n)  O(n)
Quick Sort         O(n log n)  O(n log n)  O(n²)       O(log n)
Heap Sort          O(n log n)  O(n log n)  O(n log n)  O(1)
Counting Sort      O(n + k)    O(n + k)    O(n + k)    O(k)
Tim Sort (Python)  O(n)        O(n log n)  O(n log n)  O(n)
```

---

## 6. Space Complexity — How Much Memory?

### 6.1 What is Space Complexity?

Space complexity measures how much extra memory an algorithm uses as input grows. It does NOT count the input itself — only the ADDITIONAL space.

```python
# O(1) space — only uses a few variables regardless of input size
def find_max(arr):
    max_val = arr[0]
    for num in arr:
        if num > max_val:
            max_val = num
    return max_val

# O(n) space — creates a new array of the same size
def reverse_array(arr):
    result = []
    for i in range(len(arr) - 1, -1, -1):
        result.append(arr[i])
    return result

# O(n) space — recursion uses stack space
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
    # Each recursive call adds a frame to the call stack
    # n calls deep → O(n) space
```

### 6.2 The Space-Time Tradeoff

Often, you can make an algorithm faster by using more memory, or use less memory by accepting slower speed.

Example: Finding duplicates in an array.

```python
# O(n²) time, O(1) space — compare every pair
def has_duplicate_slow(arr):
    for i in range(len(arr)):
        for j in range(i + 1, len(arr)):
            if arr[i] == arr[j]:
                return True
    return False

# O(n) time, O(n) space — use a set to remember what we've seen
def has_duplicate_fast(arr):
    seen = set()
    for num in arr:
        if num in seen:
            return True
        seen.add(num)
    return False
```

The second version is much faster but uses extra memory. This tradeoff appears constantly in DSA.

---

## 7. Recursion — Functions That Call Themselves

### 7.1 What is Recursion?

Recursion is when a function calls itself to solve a smaller version of the same problem. It keeps calling itself with smaller and smaller inputs until it reaches a **base case** — a simple case it can solve directly without calling itself.

**Analogy:** Imagine you're in a line and want to know your position. You ask the person in front of you what their position is. They ask the person in front of them. This continues until the first person says "I'm position 1." Then each person adds 1 to the answer they received and passes it back.

### 7.2 The Two Essential Parts

Every recursive function MUST have:
1. **Base case:** When to stop. Without this, the function calls itself forever (infinite recursion → crash).
2. **Recursive case:** The function calls itself with a smaller/simpler input.

```python
def countdown(n):
    # Base case
    if n <= 0:
        print("Done!")
        return

    # Recursive case
    print(n)
    countdown(n - 1)    # calls itself with a smaller number

countdown(5)
# Output: 5, 4, 3, 2, 1, Done!
```

### 7.3 How Recursion Works — The Call Stack

When a function calls itself, the computer saves its current state on a "call stack" and starts executing the new call. When the new call finishes, it returns to where it left off.

```python
def factorial(n):
    if n <= 1:         # base case
        return 1
    return n * factorial(n - 1)  # recursive case

# factorial(4) works like this:
# factorial(4) = 4 * factorial(3)
#                     factorial(3) = 3 * factorial(2)
#                                        factorial(2) = 2 * factorial(1)
#                                                            factorial(1) = 1  ← base case!
#                                        factorial(2) = 2 * 1 = 2
#                     factorial(3) = 3 * 2 = 6
# factorial(4) = 4 * 6 = 24
```

### 7.4 Recursion vs Iteration

Any recursive solution can be converted to an iterative one (using loops) and vice versa. Some problems are much more naturally expressed recursively (tree traversals, divide and conquer), while others are more natural iteratively (simple loops).

```python
# Iterative factorial
def factorial_iterative(n):
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

# Recursive factorial
def factorial_recursive(n):
    if n <= 1:
        return 1
    return n * factorial_recursive(n - 1)
```

---

## 8. Recursion Patterns and Practice

### 8.1 Pattern 1: Linear Recursion

Process one element and recurse on the rest.

```python
def sum_array(arr):
    """Sum all elements in an array."""
    if not arr:           # base case: empty array
        return 0
    return arr[0] + sum_array(arr[1:])   # first element + sum of rest

# sum_array([1, 2, 3, 4])
# = 1 + sum_array([2, 3, 4])
# = 1 + 2 + sum_array([3, 4])
# = 1 + 2 + 3 + sum_array([4])
# = 1 + 2 + 3 + 4 + sum_array([])
# = 1 + 2 + 3 + 4 + 0 = 10
```

### 8.2 Pattern 2: Binary Recursion (Divide and Conquer)

Split the problem in two, solve each half, combine the results.

```python
def fibonacci(n):
    """Find the nth Fibonacci number."""
    if n <= 1:             # base cases
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)   # two recursive calls

# fibonacci(5) = fibonacci(4) + fibonacci(3)
#              = (fib(3) + fib(2)) + (fib(2) + fib(1))
# ... and so on
# Warning: This is O(2ⁿ) — very slow! We'll fix it with dynamic programming later.
```

### 8.3 Pattern 3: Tree Recursion / Exploration

Explore all possibilities by branching at each step.

```python
def generate_parentheses(n):
    """Generate all valid combinations of n pairs of parentheses."""
    result = []

    def backtrack(current, open_count, close_count):
        if len(current) == 2 * n:     # base case: we've placed all parentheses
            result.append(current)
            return

        if open_count < n:             # can still add '('
            backtrack(current + '(', open_count + 1, close_count)

        if close_count < open_count:   # can add ')' if we have unmatched '('
            backtrack(current + ')', open_count, close_count + 1)

    backtrack('', 0, 0)
    return result

# generate_parentheses(3)
# ['((()))', '(()())', '(())()', '()(())', '()()()']
```

---

# PART 2: LINEAR DATA STRUCTURES

---

## 9. Arrays and Lists

### 9.1 What is an Array?

An array is a contiguous block of memory storing elements of the same type, accessible by index. In Python, the `list` type functions as a dynamic array.

```python
# Creating arrays/lists
numbers = [10, 20, 30, 40, 50]
empty = []
mixed = [1, "hello", 3.14, True]  # Python lists can hold mixed types

# Accessing by index (0-based: first element is index 0)
numbers[0]     # 10 (first element)
numbers[2]     # 30 (third element)
numbers[-1]    # 50 (last element)
numbers[-2]    # 40 (second to last)

# Slicing
numbers[1:3]   # [20, 30] (index 1 up to but not including 3)
numbers[:3]    # [10, 20, 30] (first 3 elements)
numbers[2:]    # [30, 40, 50] (from index 2 to end)
numbers[::2]   # [10, 30, 50] (every other element)
numbers[::-1]  # [50, 40, 30, 20, 10] (reversed)
```

### 9.2 Common Operations and Their Complexity

```python
arr = [1, 2, 3, 4, 5]

# O(1) — Access by index
arr[2]                     # 3

# O(1) amortized — Append to end
arr.append(6)              # [1, 2, 3, 4, 5, 6]

# O(n) — Insert at position (shifts all elements after)
arr.insert(2, 99)          # [1, 2, 99, 3, 4, 5, 6]

# O(n) — Remove by value (searches then shifts)
arr.remove(99)             # [1, 2, 3, 4, 5, 6]

# O(1) — Pop from end
arr.pop()                  # returns 6, arr is now [1, 2, 3, 4, 5]

# O(n) — Pop from beginning or middle
arr.pop(0)                 # returns 1, arr is now [2, 3, 4, 5]

# O(n) — Search
3 in arr                   # True
arr.index(3)               # 1 (index of value 3)

# O(n) — Length
len(arr)                   # 4

# O(n log n) — Sort
arr.sort()                 # sorts in place
sorted(arr)                # returns new sorted list
```

### 9.3 List Comprehensions

A Python-specific shorthand for creating and transforming lists:

```python
# Create a list of squares
squares = [x**2 for x in range(10)]
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Filter: only even numbers
evens = [x for x in range(20) if x % 2 == 0]
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# Transform: uppercase strings
words = ["hello", "world"]
upper = [w.upper() for w in words]
# ["HELLO", "WORLD"]

# 2D: create a matrix
matrix = [[0] * 3 for _ in range(3)]
# [[0, 0, 0], [0, 0, 0], [0, 0, 0]]
```

### 9.4 Common Array Problems and Techniques

**Reverse an array in-place (Two Pointer):**

```python
def reverse(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1
    return arr
```

**Find two numbers that sum to a target:**

```python
def two_sum(nums, target):
    """Return indices of two numbers that add up to target."""
    seen = {}  # value → index
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []

# two_sum([2, 7, 11, 15], 9)  →  [0, 1]  (because 2 + 7 = 9)
```

**Find the maximum subarray sum (Kadane's Algorithm):**

```python
def max_subarray_sum(nums):
    """Find the contiguous subarray with the largest sum."""
    max_sum = current_sum = nums[0]
    for num in nums[1:]:
        current_sum = max(num, current_sum + num)
        # Either start fresh with this number, or extend the current subarray
        max_sum = max(max_sum, current_sum)
    return max_sum

# max_subarray_sum([-2, 1, -3, 4, -1, 2, 1, -5, 4])  →  6
# (the subarray [4, -1, 2, 1] has the largest sum)
```

---

## 10. Strings

### 10.1 String Basics

Strings are sequences of characters. In Python, they're immutable — you can't change a string after creating it. Any "modification" creates a new string.

```python
s = "hello world"

# Accessing characters
s[0]        # 'h'
s[-1]       # 'd'
s[0:5]      # 'hello'

# Common methods
s.upper()           # 'HELLO WORLD'
s.lower()           # 'hello world'
s.split()           # ['hello', 'world']
s.split('o')        # ['hell', ' w', 'rld']
s.replace('o', '0') # 'hell0 w0rld'
s.strip()           # removes leading/trailing whitespace
s.startswith('he')  # True
s.endswith('ld')    # True
s.find('world')     # 6 (index where 'world' starts)
s.count('l')        # 3
''.join(['a','b'])  # 'ab'
s.isalpha()         # False (contains space)
s.isdigit()         # False
ord('a')            # 97 (ASCII value)
chr(97)             # 'a' (character from ASCII)
```

### 10.2 String Immutability — Why It Matters for Performance

```python
# BAD: O(n²) — each concatenation creates a new string
result = ""
for char in some_large_list:
    result += char  # creates a new string every time!

# GOOD: O(n) — join at the end
parts = []
for char in some_large_list:
    parts.append(char)
result = "".join(parts)
```

### 10.3 Common String Problems

**Check if a string is a palindrome:**

```python
def is_palindrome(s):
    """Check if a string reads the same forwards and backwards."""
    s = s.lower()
    left, right = 0, len(s) - 1
    while left < right:
        # Skip non-alphanumeric characters
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True

# is_palindrome("A man, a plan, a canal: Panama")  →  True
```

**Check if two strings are anagrams:**

```python
from collections import Counter

def is_anagram(s1, s2):
    return Counter(s1) == Counter(s2)

# is_anagram("listen", "silent")  →  True
```

**Find the longest substring without repeating characters:**

```python
def longest_unique_substring(s):
    seen = {}        # character → most recent index
    start = 0        # start of current window
    max_length = 0

    for end, char in enumerate(s):
        if char in seen and seen[char] >= start:
            start = seen[char] + 1   # shrink window past the duplicate
        seen[char] = end
        max_length = max(max_length, end - start + 1)

    return max_length

# longest_unique_substring("abcabcbb")  →  3 ("abc")
```

---

## 11. Linked Lists

### 11.1 What is a Linked List?

A linked list is a sequence of nodes where each node contains data and a reference (pointer) to the next node.

```
[10 | →] → [20 | →] → [30 | →] → [40 | None]
 head                                 tail
```

Unlike arrays, linked list elements are NOT stored contiguously in memory. Each node can be anywhere in memory — it just has a pointer to the next one.

### 11.2 Why Use Linked Lists?

**Advantages over arrays:**
- O(1) insertion/deletion at the beginning (arrays are O(n))
- O(1) insertion/deletion anywhere if you have a reference to the node
- Dynamic size — no need to resize

**Disadvantages:**
- O(n) access by index (arrays are O(1))
- Extra memory for pointers
- No cache friendliness (nodes scattered in memory)

### 11.3 Implementation

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class LinkedList:
    def __init__(self):
        self.head = None

    def prepend(self, val):
        """Add to the beginning — O(1)."""
        new_node = ListNode(val)
        new_node.next = self.head
        self.head = new_node

    def append(self, val):
        """Add to the end — O(n) (O(1) if we track the tail)."""
        new_node = ListNode(val)
        if not self.head:
            self.head = new_node
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = new_node

    def delete(self, val):
        """Delete first occurrence of val — O(n)."""
        if not self.head:
            return
        if self.head.val == val:
            self.head = self.head.next
            return
        current = self.head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
                return
            current = current.next

    def search(self, val):
        """Search for a value — O(n)."""
        current = self.head
        while current:
            if current.val == val:
                return True
            current = current.next
        return False

    def display(self):
        """Print the list."""
        elements = []
        current = self.head
        while current:
            elements.append(str(current.val))
            current = current.next
        print(" → ".join(elements))
```

### 11.4 Essential Linked List Techniques

**Reverse a linked list (THE most common linked list problem):**

```python
def reverse_list(head):
    prev = None
    current = head
    while current:
        next_node = current.next   # save next
        current.next = prev        # reverse the link
        prev = current             # move prev forward
        current = next_node        # move current forward
    return prev

# Before: 1 → 2 → 3 → 4 → None
# After:  4 → 3 → 2 → 1 → None
```

**Detect a cycle (Floyd's Algorithm):**

```python
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next           # moves 1 step
        fast = fast.next.next      # moves 2 steps
        if slow == fast:           # they meet → cycle exists
            return True
    return False
```

**Find the middle node:**

```python
def find_middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow  # when fast reaches the end, slow is at the middle
```

**Merge two sorted linked lists:**

```python
def merge_sorted(l1, l2):
    dummy = ListNode(0)
    current = dummy

    while l1 and l2:
        if l1.val <= l2.val:
            current.next = l1
            l1 = l1.next
        else:
            current.next = l2
            l2 = l2.next
        current = current.next

    current.next = l1 or l2  # attach remaining nodes
    return dummy.next
```

---

## 12. Stacks

### 12.1 What is a Stack?

A stack is a **Last-In, First-Out (LIFO)** data structure. The last element added is the first one removed. Think of a stack of plates — you add plates to the top and remove from the top.

### 12.2 Operations

```python
# Python lists can be used as stacks
stack = []

stack.append(10)    # push: add to top    → [10]
stack.append(20)    # push                → [10, 20]
stack.append(30)    # push                → [10, 20, 30]

stack[-1]           # peek: see top       → 30 (doesn't remove)
stack.pop()         # pop: remove top     → 30, stack is [10, 20]
len(stack)          # size                → 2
not stack           # is empty?           → False
```

All operations are **O(1)**.

### 12.3 When to Use Stacks

- **Matching pairs:** Parentheses matching, HTML tag matching
- **Undo/Redo:** Each action is pushed onto the stack; undo pops the last action
- **Function calls:** The computer uses a call stack to manage function calls
- **DFS traversal:** Depth-first search uses a stack (explicitly or via recursion)
- **Expression evaluation:** Converting and evaluating mathematical expressions

### 12.4 Classic Problem: Valid Parentheses

```python
def is_valid_parentheses(s):
    """Check if brackets are properly matched and nested."""
    stack = []
    matching = {')': '(', ']': '[', '}': '{'}

    for char in s:
        if char in '([{':
            stack.append(char)
        elif char in ')]}':
            if not stack or stack[-1] != matching[char]:
                return False
            stack.pop()

    return len(stack) == 0

# is_valid_parentheses("({[]})")  →  True
# is_valid_parentheses("([)]")    →  False
# is_valid_parentheses("((")      →  False
```

### 12.5 Next Greater Element (Monotonic Stack)

```python
def next_greater_element(nums):
    """For each element, find the next element that is greater."""
    result = [-1] * len(nums)
    stack = []  # stores indices

    for i in range(len(nums)):
        while stack and nums[i] > nums[stack[-1]]:
            idx = stack.pop()
            result[idx] = nums[i]
        stack.append(i)

    return result

# next_greater_element([4, 5, 2, 10, 8])
# → [5, 10, 10, -1, -1]
```

---

## 13. Queues

### 13.1 What is a Queue?

A queue is a **First-In, First-Out (FIFO)** data structure. The first element added is the first one removed. Think of a line at a store — the first person in line is served first.

### 13.2 Implementation

```python
from collections import deque

queue = deque()

queue.append(10)       # enqueue: add to back    → [10]
queue.append(20)       # enqueue                 → [10, 20]
queue.append(30)       # enqueue                 → [10, 20, 30]

queue[0]               # peek: see front         → 10
queue.popleft()        # dequeue: remove front   → 10, queue is [20, 30]
len(queue)             # size                    → 2
```

**Why `deque` and not `list`?** `list.pop(0)` is O(n) because it shifts all elements. `deque.popleft()` is O(1). Always use `deque` for queues.

### 13.3 When to Use Queues

- **BFS traversal:** Breadth-first search uses a queue
- **Level-order tree traversal**
- **Task scheduling:** First-come, first-served
- **Buffering:** Print queues, message queues

### 13.4 BFS Using a Queue (Preview)

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)

    while queue:
        node = queue.popleft()
        print(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

---

## 14. Hash Maps (Dictionaries)

### 14.1 What is a Hash Map?

A hash map stores **key-value pairs** and provides near-instant (O(1) average) lookup, insertion, and deletion by key.

It works by running the key through a **hash function** that converts it to an index in an internal array. When you look up a key, the hash function instantly tells you where to find the value.

```python
# Python's dict IS a hash map
phone_book = {
    "Alice": "555-0100",
    "Bob": "555-0200",
    "Charlie": "555-0300",
}

# O(1) — Lookup
phone_book["Alice"]              # "555-0100"

# O(1) — Insert/Update
phone_book["Dave"] = "555-0400"

# O(1) — Delete
del phone_book["Bob"]

# O(1) — Check if key exists
"Alice" in phone_book            # True

# O(n) — Iterate
for name, number in phone_book.items():
    print(f"{name}: {number}")

# Useful methods
phone_book.get("Eve", "Not found")   # "Not found" (default if key missing)
phone_book.keys()                     # all keys
phone_book.values()                   # all values
```

### 14.2 Counter (Frequency Counting)

The most common hash map pattern in interviews:

```python
from collections import Counter

# Count character frequencies
text = "hello world"
freq = Counter(text)
# Counter({'l': 3, 'o': 2, 'h': 1, 'e': 1, ' ': 1, 'w': 1, 'r': 1, 'd': 1})

freq.most_common(3)  # [('l', 3), ('o', 2), ('h', 1)]
```

### 14.3 defaultdict

Automatically initializes missing keys:

```python
from collections import defaultdict

# Group words by their first letter
words = ["apple", "banana", "avocado", "blueberry", "cherry", "apricot"]
groups = defaultdict(list)
for word in words:
    groups[word[0]].append(word)

# {'a': ['apple', 'avocado', 'apricot'], 'b': ['banana', 'blueberry'], 'c': ['cherry']}
```

### 14.4 Classic Hash Map Problems

**Group Anagrams:**

```python
def group_anagrams(strs):
    groups = defaultdict(list)
    for s in strs:
        key = tuple(sorted(s))  # anagrams have the same sorted form
        groups[key].append(s)
    return list(groups.values())

# group_anagrams(["eat","tea","tan","ate","nat","bat"])
# → [["eat","tea","ate"], ["tan","nat"], ["bat"]]
```

**Two Sum (using hash map):**

```python
def two_sum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
```

---

## 15. Sets

### 15.1 What is a Set?

A set is an unordered collection of unique elements. It supports O(1) average lookup, add, and remove — like a hash map but with only keys, no values.

```python
s = {1, 2, 3, 4, 5}

s.add(6)          # add element
s.remove(3)       # remove (raises error if missing)
s.discard(10)     # remove (no error if missing)
3 in s            # check membership — O(1)
len(s)            # number of elements

# Set operations
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

a | b              # union:        {1, 2, 3, 4, 5, 6}
a & b              # intersection:  {3, 4}
a - b              # difference:    {1, 2}
a ^ b              # symmetric diff: {1, 2, 5, 6}
a.issubset(b)      # is a ⊆ b?     False
```

### 15.2 When to Use Sets

- Removing duplicates: `list(set(my_list))`
- Fast membership testing: `if item in my_set`
- Finding common/different elements between collections
- Tracking visited nodes in graph algorithms

---

# PART 3: NON-LINEAR DATA STRUCTURES

---

## 16. Trees — Concepts and Terminology

### 16.1 What is a Tree?

A tree is a hierarchical data structure with a **root** node at the top and **child** nodes branching below. Every node (except the root) has exactly one parent.

```
        1          ← root
       / \
      2   3        ← children of 1
     / \   \
    4   5   6      ← leaves (no children)
```

### 16.2 Terminology

- **Root:** The top node (1)
- **Parent:** Node directly above (2 is parent of 4 and 5)
- **Child:** Node directly below (4 and 5 are children of 2)
- **Leaf:** Node with no children (4, 5, 6)
- **Edge:** Connection between two nodes
- **Depth:** Distance from root to a node (root is depth 0)
- **Height:** Distance from a node to its deepest leaf (root's height = tree's height)
- **Subtree:** A node and all its descendants
- **Level:** All nodes at the same depth

---

## 17. Binary Trees

### 17.1 What is a Binary Tree?

A binary tree is a tree where each node has **at most 2 children** — left child and right child.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

### 17.2 Tree Traversals

There are four ways to visit all nodes:

**Inorder (Left → Root → Right):**

```python
def inorder(root):
    if not root:
        return []
    return inorder(root.left) + [root.val] + inorder(root.right)
# For a BST, this gives sorted order!
```

**Preorder (Root → Left → Right):**

```python
def preorder(root):
    if not root:
        return []
    return [root.val] + preorder(root.left) + preorder(root.right)
# Useful for: copying a tree, serialization
```

**Postorder (Left → Right → Root):**

```python
def postorder(root):
    if not root:
        return []
    return postorder(root.left) + postorder(root.right) + [root.val]
# Useful for: deleting a tree, evaluating expressions
```

**Level-order (BFS — level by level):**

```python
from collections import deque

def level_order(root):
    if not root:
        return []
    result = []
    queue = deque([root])
    while queue:
        level = []
        for _ in range(len(queue)):
            node = queue.popleft()
            level.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        result.append(level)
    return result

# Returns: [[1], [2, 3], [4, 5, 6]]
```

### 17.3 Common Binary Tree Problems

**Maximum depth:**

```python
def max_depth(root):
    if not root:
        return 0
    return 1 + max(max_depth(root.left), max_depth(root.right))
```

**Check if two trees are identical:**

```python
def is_same_tree(p, q):
    if not p and not q:
        return True
    if not p or not q:
        return False
    return (p.val == q.val and
            is_same_tree(p.left, q.left) and
            is_same_tree(p.right, q.right))
```

**Invert (mirror) a binary tree:**

```python
def invert_tree(root):
    if not root:
        return None
    root.left, root.right = invert_tree(root.right), invert_tree(root.left)
    return root
```

---

## 18. Binary Search Trees (BST)

### 18.1 What is a BST?

A BST is a binary tree with a special property: for every node, all values in its left subtree are **less than** the node, and all values in its right subtree are **greater than** the node.

```
        8
       / \
      3   10
     / \    \
    1   6    14
       / \   /
      4   7 13
```

This property makes searching very efficient — at each node, you know which half of the tree to search.

### 18.2 Operations

```python
def search_bst(root, target):
    """Search for a value — O(log n) average, O(n) worst."""
    if not root:
        return None
    if target == root.val:
        return root
    elif target < root.val:
        return search_bst(root.left, target)
    else:
        return search_bst(root.right, target)

def insert_bst(root, val):
    """Insert a value — O(log n) average."""
    if not root:
        return TreeNode(val)
    if val < root.val:
        root.left = insert_bst(root.left, val)
    else:
        root.right = insert_bst(root.right, val)
    return root
```

### 18.3 BST Validation

```python
def is_valid_bst(root, min_val=float('-inf'), max_val=float('inf')):
    if not root:
        return True
    if root.val <= min_val or root.val >= max_val:
        return False
    return (is_valid_bst(root.left, min_val, root.val) and
            is_valid_bst(root.right, root.val, max_val))
```

---

## 19. AVL Trees (Self-Balancing BSTs)

### 19.1 The Problem with BSTs

If you insert sorted data into a BST, it becomes a straight line (like a linked list), and all operations become O(n) instead of O(log n).

```
Insert 1, 2, 3, 4, 5:
    1
     \
      2
       \
        3
         \
          4
           \
            5     ← This is a "degenerate" tree — O(n) search
```

### 19.2 The Solution: Self-Balancing

AVL trees automatically balance themselves after every insertion/deletion by performing **rotations**. They maintain the property that for every node, the heights of its left and right subtrees differ by at most 1.

In practice, Python doesn't have a built-in AVL tree. Use `sortedcontainers.SortedList` or a balanced BST library for this functionality.

---

## 20. Heaps and Priority Queues

### 20.1 What is a Heap?

A heap is a complete binary tree where every parent is smaller (min-heap) or larger (max-heap) than its children.

**Min-heap:** The smallest element is always at the root.
**Max-heap:** The largest element is always at the root.

```
Min-heap:           Max-heap:
     1                   9
    / \                 / \
   3   5               7   8
  / \                 / \
 7   4               3   5
```

### 20.2 Why Heaps?

Heaps efficiently support:
- **Get min/max:** O(1) — just look at the root
- **Insert:** O(log n) — add at bottom, "bubble up"
- **Remove min/max:** O(log n) — replace root, "bubble down"

### 20.3 Python's heapq Module

Python provides a min-heap implementation:

```python
import heapq

# Create a heap
heap = []
heapq.heappush(heap, 5)
heapq.heappush(heap, 3)
heapq.heappush(heap, 8)
heapq.heappush(heap, 1)
# heap is now [1, 3, 8, 5] (min-heap)

heapq.heappop(heap)   # 1 (removes and returns smallest)
heap[0]                # 3 (peek at smallest without removing)

# Create from a list — O(n)
nums = [5, 3, 8, 1, 4]
heapq.heapify(nums)
# nums is now a valid min-heap

# For a MAX-heap, negate the values:
max_heap = []
heapq.heappush(max_heap, -5)   # push -5
heapq.heappush(max_heap, -3)
val = -heapq.heappop(max_heap) # pop and negate → 5 (the largest)

# Get the K largest/smallest
heapq.nlargest(3, [5, 1, 8, 3, 2])   # [8, 5, 3]
heapq.nsmallest(3, [5, 1, 8, 3, 2])  # [1, 2, 3]
```

### 20.4 Classic Heap Problems

**Kth Largest Element:**

```python
def find_kth_largest(nums, k):
    """Find the kth largest element — O(n log k)."""
    min_heap = []
    for num in nums:
        heapq.heappush(min_heap, num)
        if len(min_heap) > k:
            heapq.heappop(min_heap)
    return min_heap[0]

# find_kth_largest([3,2,1,5,6,4], 2)  →  5
```

**Merge K Sorted Lists:**

```python
def merge_k_sorted(lists):
    """Merge k sorted linked lists — O(n log k)."""
    heap = []
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, (lst.val, i, lst))

    dummy = ListNode(0)
    current = dummy

    while heap:
        val, i, node = heapq.heappop(heap)
        current.next = node
        current = current.next
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))

    return dummy.next
```

---

## 21. Tries (Prefix Trees)

### 21.1 What is a Trie?

A trie is a tree-like data structure used for storing strings, where each node represents a character. It's incredibly efficient for prefix-based operations like autocomplete.

```
        root
       / | \
      a  b  c
     /   |
    p    a
   / \   |
  p   r  t
  |
  l
  |
  e
  → stores: "apple", "app", "art", "bat"
```

### 21.2 Implementation

```python
class TrieNode:
    def __init__(self):
        self.children = {}    # character → TrieNode
        self.is_end = False   # marks end of a complete word

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        """Insert a word — O(m) where m is word length."""
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True

    def search(self, word):
        """Check if the exact word exists — O(m)."""
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end

    def starts_with(self, prefix):
        """Check if any word starts with the prefix — O(m)."""
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

---

## 22. Graphs — Concepts and Terminology

### 22.1 What is a Graph?

A graph is a set of **nodes** (vertices) connected by **edges**. Unlike trees, graphs can have cycles, multiple connections, and no hierarchy.

```
    A --- B
    |   / |
    |  /  |
    C --- D
```

### 22.2 Types of Graphs

**Directed vs Undirected:** In a directed graph, edges have a direction (A→B ≠ B→A). In an undirected graph, edges go both ways.

**Weighted vs Unweighted:** In a weighted graph, edges have a cost/distance. In an unweighted graph, all edges are equal.

**Cyclic vs Acyclic:** A cycle is a path that starts and ends at the same node. A DAG (Directed Acyclic Graph) has no cycles.

**Connected vs Disconnected:** A connected graph has a path between every pair of nodes. A disconnected graph has isolated groups.

### 22.3 Terminology

- **Vertex (Node):** A point in the graph
- **Edge:** A connection between two vertices
- **Degree:** Number of edges connected to a node
- **Path:** A sequence of edges from one node to another
- **Cycle:** A path that returns to the starting node
- **Neighbor (Adjacent):** Nodes directly connected by an edge

---

## 23. Graph Representations

### 23.1 Adjacency List (Most Common)

Each node stores a list of its neighbors. Memory-efficient for sparse graphs.

```python
# Using a dictionary
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'C', 'D'],
    'C': ['A', 'B', 'D'],
    'D': ['B', 'C'],
}

# Using defaultdict
from collections import defaultdict
graph = defaultdict(list)

# Adding edges (undirected)
edges = [('A', 'B'), ('A', 'C'), ('B', 'D'), ('C', 'D')]
for u, v in edges:
    graph[u].append(v)
    graph[v].append(u)

# Weighted graph
weighted_graph = {
    'A': [('B', 5), ('C', 3)],
    'B': [('A', 5), ('D', 2)],
    'C': [('A', 3), ('D', 7)],
    'D': [('B', 2), ('C', 7)],
}
```

### 23.2 Adjacency Matrix

A 2D grid where `matrix[i][j] = 1` if there's an edge from i to j. Memory-intensive for sparse graphs but O(1) edge lookup.

```python
# 4 nodes: A=0, B=1, C=2, D=3
matrix = [
    [0, 1, 1, 0],  # A connects to B, C
    [1, 0, 1, 1],  # B connects to A, C, D
    [1, 1, 0, 1],  # C connects to A, B, D
    [0, 1, 1, 0],  # D connects to B, C
]
```

### 23.3 Edge List

A simple list of all edges. Useful for algorithms like Kruskal's.

```python
edges = [('A', 'B', 5), ('A', 'C', 3), ('B', 'D', 2), ('C', 'D', 7)]
# Each tuple: (from, to, weight)
```

---

## 24. Union-Find (Disjoint Set Union)

### 24.1 What is Union-Find?

Union-Find tracks which elements belong to which groups (sets). It supports two operations efficiently:
- **Find:** Which group does this element belong to?
- **Union:** Merge two groups together.

### 24.2 Implementation

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        """Find the root of x's group — with path compression."""
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        """Merge the groups of x and y — with union by rank."""
        px, py = self.find(x), self.find(y)
        if px == py:
            return False  # already in the same group
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        return True

    def connected(self, x, y):
        return self.find(x) == self.find(y)
```

### 24.3 When to Use Union-Find

- Detecting cycles in graphs
- Finding connected components
- Kruskal's algorithm for minimum spanning trees
- Network connectivity problems

---

# PART 4: CORE ALGORITHMS

---

## 25. Searching Algorithms

### 25.1 Linear Search — O(n)

Check every element one by one. Works on unsorted data.

```python
def linear_search(arr, target):
    for i, val in enumerate(arr):
        if val == target:
            return i
    return -1
```

### 25.2 Binary Search — O(log n)

Requires sorted data. Eliminates half the remaining elements at each step.

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = left + (right - left) // 2  # avoids integer overflow
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1
```

**Why `left + (right - left) // 2` instead of `(left + right) // 2`?** In languages with fixed-size integers, `left + right` can overflow. In Python this isn't an issue, but it's good practice and expected in interviews.

---

## 26. Sorting Algorithms

### 26.1 Bubble Sort — O(n²)

Repeatedly swap adjacent elements if they're in the wrong order. Simple but slow.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:   # optimization: stop if no swaps
            break
    return arr
```

### 26.2 Merge Sort — O(n log n)

Divide the array in half, sort each half, merge them back together. Consistent performance, uses O(n) extra space.

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

### 26.3 Quick Sort — O(n log n) average

Pick a pivot, partition the array into elements less than and greater than the pivot, recursively sort each partition.

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr

    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]

    return quick_sort(left) + middle + quick_sort(right)
```

### 26.4 When to Use Which

- **Python's built-in `sorted()` and `.sort()`:** Use Tim Sort (hybrid). Always use this in practice.
- **Merge Sort:** When you need stable, consistent O(n log n). Good for linked lists.
- **Quick Sort:** Usually fastest in practice. Default in many languages.
- **Counting/Radix Sort:** When values are in a limited range (e.g., ages 0-150).

---

## 27. Two Pointers Technique

### 27.1 The Idea

Use two pointers (indices) that move through the data, usually from opposite ends or at different speeds. Reduces many O(n²) problems to O(n).

### 27.2 Pattern: Opposite Direction

```python
def two_sum_sorted(arr, target):
    """Find two numbers in a SORTED array that sum to target."""
    left, right = 0, len(arr) - 1
    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return [left, right]
        elif current_sum < target:
            left += 1        # need a bigger sum → move left pointer right
        else:
            right -= 1       # need a smaller sum → move right pointer left
    return []
```

### 27.3 Pattern: Same Direction (Fast & Slow)

```python
def remove_duplicates(arr):
    """Remove duplicates from sorted array in-place. Return new length."""
    if not arr:
        return 0
    slow = 0
    for fast in range(1, len(arr)):
        if arr[fast] != arr[slow]:
            slow += 1
            arr[slow] = arr[fast]
    return slow + 1
```

### 27.4 Three Sum

```python
def three_sum(nums):
    """Find all unique triplets that sum to zero."""
    nums.sort()
    result = []

    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i-1]:
            continue  # skip duplicates

        left, right = i + 1, len(nums) - 1
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            if total == 0:
                result.append([nums[i], nums[left], nums[right]])
                while left < right and nums[left] == nums[left+1]:
                    left += 1
                while left < right and nums[right] == nums[right-1]:
                    right -= 1
                left += 1
                right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1

    return result
```

---

## 28. Sliding Window Technique

### 28.1 The Idea

Maintain a "window" (subarray) that slides through the data. Instead of recalculating everything from scratch for each window position, you add the new element and remove the old one. Turns O(n×k) into O(n).

### 28.2 Fixed-Size Window

```python
def max_sum_subarray(arr, k):
    """Find the maximum sum of any subarray of size k."""
    window_sum = sum(arr[:k])
    max_sum = window_sum

    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]  # add new element, remove old
        max_sum = max(max_sum, window_sum)

    return max_sum
```

### 28.3 Variable-Size Window

```python
def min_subarray_length(target, nums):
    """Find the smallest subarray with sum >= target."""
    left = 0
    current_sum = 0
    min_length = float('inf')

    for right in range(len(nums)):
        current_sum += nums[right]

        while current_sum >= target:
            min_length = min(min_length, right - left + 1)
            current_sum -= nums[left]
            left += 1

    return min_length if min_length != float('inf') else 0
```

---

## 29. Binary Search — Beyond Arrays

### 29.1 Binary Search on Answer Space

Binary search can be applied whenever you have a monotonic (consistently increasing or decreasing) function and need to find a threshold.

```python
def min_eating_speed(piles, h):
    """Koko eats bananas. Find minimum speed to eat all in h hours."""
    def can_finish(speed):
        hours = sum((pile + speed - 1) // speed for pile in piles)
        return hours <= h

    left, right = 1, max(piles)
    while left < right:
        mid = (left + right) // 2
        if can_finish(mid):
            right = mid
        else:
            left = mid + 1
    return left
```

### 29.2 Finding the Boundary

```python
def find_first_true(arr):
    """Find the first True in [False, False, ..., True, True, ...]."""
    left, right = 0, len(arr) - 1
    result = -1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid]:
            result = mid
            right = mid - 1
        else:
            left = mid + 1
    return result
```

---

## 30. BFS (Breadth-First Search)

### 30.1 The Idea

BFS explores a graph **level by level**. It visits all nodes at distance 1 first, then distance 2, then 3, and so on. It uses a **queue**.

BFS guarantees the shortest path in an unweighted graph.

### 30.2 Implementation

```python
from collections import deque

def bfs(graph, start):
    visited = set([start])
    queue = deque([start])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

    return order
```

### 30.3 Shortest Path in Unweighted Graph

```python
def shortest_path(graph, start, end):
    queue = deque([(start, [start])])
    visited = set([start])

    while queue:
        node, path = queue.popleft()
        if node == end:
            return path
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))

    return None  # no path exists
```

### 30.4 BFS on a Grid (Matrix)

```python
def num_islands(grid):
    """Count the number of islands in a grid of '1's and '0's."""
    if not grid:
        return 0

    rows, cols = len(grid), len(grid[0])
    count = 0

    def bfs(r, c):
        queue = deque([(r, c)])
        grid[r][c] = '0'
        while queue:
            row, col = queue.popleft()
            for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
                nr, nc = row + dr, col + dc
                if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == '1':
                    grid[nr][nc] = '0'
                    queue.append((nr, nc))

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                bfs(r, c)
                count += 1

    return count
```

---

## 31. DFS (Depth-First Search)

### 31.1 The Idea

DFS explores as deep as possible before backtracking. It uses a **stack** (or recursion, which implicitly uses the call stack).

### 31.2 Iterative DFS

```python
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]

    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            print(node)
            for neighbor in graph[node]:
                if neighbor not in visited:
                    stack.append(neighbor)
```

### 31.3 Recursive DFS

```python
def dfs_recursive(graph, node, visited=None):
    if visited is None:
        visited = set()
    visited.add(node)
    print(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)
```

### 31.4 When to Use BFS vs DFS

| | BFS | DFS |
|---|---|---|
| **Uses** | Queue | Stack / Recursion |
| **Pattern** | Level by level | Deep then backtrack |
| **Shortest path** | Yes (unweighted) | No |
| **Memory** | O(width) | O(depth) |
| **Best for** | Shortest path, level-order | Connectivity, cycles, paths, backtracking |

---

## 32. Topological Sort

### 32.1 What is it?

Topological sort orders the vertices of a DAG (Directed Acyclic Graph) so that for every edge u→v, u comes before v. Think of it as ordering tasks where some tasks depend on others.

### 32.2 Kahn's Algorithm (BFS-based)

```python
from collections import deque, defaultdict

def topological_sort(num_nodes, edges):
    graph = defaultdict(list)
    in_degree = [0] * num_nodes

    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1

    queue = deque([i for i in range(num_nodes) if in_degree[i] == 0])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    if len(order) != num_nodes:
        return []  # cycle detected
    return order
```

---

## 33. Shortest Path Algorithms

### 33.1 Dijkstra's Algorithm — O((V + E) log V)

Finds the shortest path from a source to all other nodes in a weighted graph with **non-negative** weights.

```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    heap = [(0, start)]

    while heap:
        dist, node = heapq.heappop(heap)
        if dist > distances[node]:
            continue
        for neighbor, weight in graph[node]:
            new_dist = dist + weight
            if new_dist < distances[neighbor]:
                distances[neighbor] = new_dist
                heapq.heappush(heap, (new_dist, neighbor))

    return distances
```

### 33.2 Bellman-Ford — O(V × E)

Handles negative weights. Detects negative cycles.

```python
def bellman_ford(num_nodes, edges, start):
    dist = [float('inf')] * num_nodes
    dist[start] = 0

    for _ in range(num_nodes - 1):
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w

    # Check for negative cycles
    for u, v, w in edges:
        if dist[u] != float('inf') and dist[u] + w < dist[v]:
            return None  # negative cycle
    return dist
```

---

## 34. Minimum Spanning Trees

### 34.1 What is an MST?

A minimum spanning tree connects all vertices in a weighted, undirected graph with the minimum total edge weight, using exactly V-1 edges and no cycles.

### 34.2 Kruskal's Algorithm — O(E log E)

Sort all edges by weight. Add the cheapest edge that doesn't create a cycle (using Union-Find).

```python
def kruskal(num_nodes, edges):
    edges.sort(key=lambda x: x[2])  # sort by weight
    uf = UnionFind(num_nodes)
    mst = []

    for u, v, w in edges:
        if uf.union(u, v):
            mst.append((u, v, w))
            if len(mst) == num_nodes - 1:
                break

    return mst
```

---

# PART 5: ADVANCED ALGORITHMIC TECHNIQUES

---

## 35. Greedy Algorithms

### 35.1 The Idea

A greedy algorithm makes the **locally optimal choice** at each step, hoping to find the globally optimal solution. It never reconsiders past choices.

Greedy works when the problem has the **greedy choice property** — a local optimum leads to a global optimum.

### 35.2 Example: Activity Selection

```python
def max_activities(activities):
    """Select maximum non-overlapping activities."""
    activities.sort(key=lambda x: x[1])  # sort by end time
    result = [activities[0]]
    last_end = activities[0][1]

    for start, end in activities[1:]:
        if start >= last_end:
            result.append((start, end))
            last_end = end

    return result
```

### 35.3 Example: Jump Game

```python
def can_jump(nums):
    """Can you reach the last index by jumping?"""
    max_reach = 0
    for i, jump in enumerate(nums):
        if i > max_reach:
            return False
        max_reach = max(max_reach, i + jump)
    return True
```

---

## 36. Dynamic Programming — Concepts

### 36.1 What is Dynamic Programming?

Dynamic Programming (DP) solves problems by breaking them into overlapping subproblems, solving each subproblem only once, and storing the results. It's like recursion with a memory.

### 36.2 When to Use DP

A problem is likely a DP problem if:
1. It asks for the **optimal** solution (maximum, minimum, longest, shortest, count of ways)
2. It can be broken into **overlapping subproblems** (the same subproblems are solved multiple times)
3. It has **optimal substructure** (the optimal solution uses optimal solutions of subproblems)

### 36.3 Two Approaches

**Top-Down (Memoization):** Start with the original problem, recursively solve subproblems, cache results.

```python
def fib_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib_memo(n-1) + fib_memo(n-2)
    return memo[n]
```

**Bottom-Up (Tabulation):** Start with the smallest subproblems, build up to the original problem using a table.

```python
def fib_table(n):
    if n <= 1:
        return n
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

### 36.4 The DP Recipe

1. **Define the state:** What does `dp[i]` (or `dp[i][j]`) represent?
2. **Find the recurrence:** How does `dp[i]` relate to previous states?
3. **Set the base case:** What are the smallest subproblems you can solve directly?
4. **Determine the order:** Which direction to fill the table?
5. **Optimize space** if possible.

---

## 37. Dynamic Programming — 1D Problems

### 37.1 Climbing Stairs

```python
def climb_stairs(n):
    """How many ways to climb n stairs taking 1 or 2 steps at a time?"""
    # dp[i] = number of ways to reach step i
    # dp[i] = dp[i-1] + dp[i-2]   (came from 1 step back or 2 steps back)
    if n <= 2:
        return n
    prev2, prev1 = 1, 2
    for i in range(3, n + 1):
        curr = prev1 + prev2
        prev2, prev1 = prev1, curr
    return prev1
```

### 37.2 House Robber

```python
def rob(nums):
    """Maximum money robbing non-adjacent houses."""
    # dp[i] = max money from houses 0..i
    # dp[i] = max(dp[i-1], dp[i-2] + nums[i])
    if not nums:
        return 0
    if len(nums) <= 2:
        return max(nums)

    prev2, prev1 = nums[0], max(nums[0], nums[1])
    for i in range(2, len(nums)):
        curr = max(prev1, prev2 + nums[i])
        prev2, prev1 = prev1, curr
    return prev1
```

### 37.3 Coin Change

```python
def coin_change(coins, amount):
    """Minimum coins needed to make the amount."""
    # dp[i] = minimum coins to make amount i
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0

    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i and dp[i - coin] != float('inf'):
                dp[i] = min(dp[i], dp[i - coin] + 1)

    return dp[amount] if dp[amount] != float('inf') else -1
```

### 37.4 Longest Increasing Subsequence

```python
def longest_increasing_subsequence(nums):
    """Length of the longest strictly increasing subsequence."""
    if not nums:
        return 0
    # dp[i] = length of LIS ending at index i
    dp = [1] * len(nums)

    for i in range(1, len(nums)):
        for j in range(i):
            if nums[j] < nums[i]:
                dp[i] = max(dp[i], dp[j] + 1)

    return max(dp)

# [10, 9, 2, 5, 3, 7, 101, 18] → 4 (the LIS is [2, 3, 7, 101])
```

---

## 38. Dynamic Programming — 2D Problems

### 38.1 Unique Paths

```python
def unique_paths(m, n):
    """Number of ways to go from top-left to bottom-right in a grid (only right/down)."""
    dp = [[1] * n for _ in range(m)]

    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]

    return dp[m-1][n-1]
```

### 38.2 Longest Common Subsequence

```python
def longest_common_subsequence(text1, text2):
    """Length of the longest subsequence common to both strings."""
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])

    return dp[m][n]

# longest_common_subsequence("abcde", "ace") → 3 ("ace")
```

### 38.3 Edit Distance

```python
def edit_distance(word1, word2):
    """Minimum operations to convert word1 to word2 (insert, delete, replace)."""
    m, n = len(word1), len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],      # delete
                    dp[i][j-1],      # insert
                    dp[i-1][j-1],    # replace
                )

    return dp[m][n]
```

---

## 39. Dynamic Programming — Classic Problems

### 39.1 0/1 Knapsack

```python
def knapsack(weights, values, capacity):
    """Maximum value you can carry given weight capacity."""
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for w in range(capacity + 1):
            dp[i][w] = dp[i-1][w]  # don't take item i
            if weights[i-1] <= w:
                dp[i][w] = max(dp[i][w], dp[i-1][w - weights[i-1]] + values[i-1])

    return dp[n][capacity]
```

### 39.2 Word Break

```python
def word_break(s, word_dict):
    """Can the string be segmented into dictionary words?"""
    dp = [False] * (len(s) + 1)
    dp[0] = True

    for i in range(1, len(s) + 1):
        for word in word_dict:
            if i >= len(word) and dp[i - len(word)] and s[i - len(word):i] == word:
                dp[i] = True
                break

    return dp[len(s)]
```

---

## 40. Backtracking

### 40.1 The Idea

Backtracking builds solutions incrementally, trying each possibility. If a partial solution can't lead to a valid solution, it **backtracks** (undoes the last choice and tries the next option).

### 40.2 Template

```python
def backtrack(state, choices, result):
    if is_solution(state):
        result.append(state.copy())
        return

    for choice in choices:
        if is_valid(choice, state):
            make_choice(choice, state)
            backtrack(state, choices, result)
            undo_choice(choice, state)  # backtrack!
```

### 40.3 Subsets

```python
def subsets(nums):
    result = []
    def backtrack(start, current):
        result.append(current[:])
        for i in range(start, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()
    backtrack(0, [])
    return result

# subsets([1,2,3]) → [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

### 40.4 Permutations

```python
def permutations(nums):
    result = []
    def backtrack(current, remaining):
        if not remaining:
            result.append(current[:])
            return
        for i in range(len(remaining)):
            current.append(remaining[i])
            backtrack(current, remaining[:i] + remaining[i+1:])
            current.pop()
    backtrack([], nums)
    return result
```

### 40.5 N-Queens

```python
def solve_n_queens(n):
    result = []
    board = [['.' ] * n for _ in range(n)]

    def is_safe(row, col):
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        for i, j in zip(range(row-1, -1, -1), range(col-1, -1, -1)):
            if board[i][j] == 'Q':
                return False
        for i, j in zip(range(row-1, -1, -1), range(col+1, n)):
            if board[i][j] == 'Q':
                return False
        return True

    def backtrack(row):
        if row == n:
            result.append([''.join(r) for r in board])
            return
        for col in range(n):
            if is_safe(row, col):
                board[row][col] = 'Q'
                backtrack(row + 1)
                board[row][col] = '.'

    backtrack(0)
    return result
```

---

## 41. Divide and Conquer

### 41.1 The Idea

Split the problem into smaller subproblems, solve each independently, and combine the results. Unlike DP, subproblems don't overlap.

### 41.2 Merge Sort (Classic Example)

Already covered in Chapter 26. Split → Sort halves → Merge.

### 41.3 Maximum Subarray (Divide and Conquer)

```python
def max_subarray_dc(nums, left, right):
    if left == right:
        return nums[left]

    mid = (left + right) // 2

    left_max = max_subarray_dc(nums, left, mid)
    right_max = max_subarray_dc(nums, mid + 1, right)

    # Max crossing subarray
    left_sum = float('-inf')
    total = 0
    for i in range(mid, left - 1, -1):
        total += nums[i]
        left_sum = max(left_sum, total)

    right_sum = float('-inf')
    total = 0
    for i in range(mid + 1, right + 1):
        total += nums[i]
        right_sum = max(right_sum, total)

    return max(left_max, right_max, left_sum + right_sum)
```

---

## 42. Bit Manipulation

### 42.1 The Basics

Computers store numbers in binary (base 2). Bit manipulation operates directly on these bits.

```python
# Binary representation
bin(10)      # '0b1010'
bin(7)       # '0b111'

# Bitwise operators
a & b        # AND:  1 only if both bits are 1
a | b        # OR:   1 if either bit is 1
a ^ b        # XOR:  1 if bits are different
~a           # NOT:  flip all bits
a << n       # Left shift: multiply by 2ⁿ
a >> n       # Right shift: divide by 2ⁿ
```

### 42.2 Common Tricks

```python
# Check if n is even/odd
n & 1 == 0    # even
n & 1 == 1    # odd

# Check if n is a power of 2
n > 0 and (n & (n - 1)) == 0

# Count set bits (number of 1s)
bin(n).count('1')

# XOR tricks:
# a ^ a = 0 (any number XOR itself is 0)
# a ^ 0 = a (any number XOR 0 is itself)

# Find the number that appears once (all others appear twice):
def single_number(nums):
    result = 0
    for num in nums:
        result ^= num
    return result
# [4, 1, 2, 1, 2] → 4  (1^1=0, 2^2=0, only 4 remains)
```

---

# PART 6: ADVANCED DATA STRUCTURES

---

## 43. Segment Trees

### 43.1 What is a Segment Tree?

A segment tree allows range queries (e.g., "sum of elements from index 2 to 7") and point updates in O(log n) time.

```python
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self.build(arr, 0, 0, self.n - 1)

    def build(self, arr, node, start, end):
        if start == end:
            self.tree[node] = arr[start]
        else:
            mid = (start + end) // 2
            self.build(arr, 2*node+1, start, mid)
            self.build(arr, 2*node+2, mid+1, end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    def update(self, idx, val, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if start == end:
            self.tree[node] = val
        else:
            mid = (start + end) // 2
            if idx <= mid:
                self.update(idx, val, 2*node+1, start, mid)
            else:
                self.update(idx, val, 2*node+2, mid+1, end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    def query(self, l, r, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if r < start or end < l:
            return 0
        if l <= start and end <= r:
            return self.tree[node]
        mid = (start + end) // 2
        return (self.query(l, r, 2*node+1, start, mid) +
                self.query(l, r, 2*node+2, mid+1, end))
```

---

## 44. Binary Indexed Trees (Fenwick Trees)

### 44.1 What is a Fenwick Tree?

A Fenwick tree provides prefix sum queries and point updates in O(log n) with less code and memory than a segment tree.

```python
class FenwickTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)

    def update(self, i, delta):
        """Add delta to index i."""
        i += 1
        while i <= self.n:
            self.tree[i] += delta
            i += i & (-i)

    def prefix_sum(self, i):
        """Sum of elements from 0 to i."""
        i += 1
        total = 0
        while i > 0:
            total += self.tree[i]
            i -= i & (-i)
        return total

    def range_sum(self, l, r):
        """Sum of elements from l to r."""
        return self.prefix_sum(r) - (self.prefix_sum(l - 1) if l > 0 else 0)
```

---

## 45. Monotonic Stack and Monotonic Queue

### 45.1 Monotonic Stack

A stack that maintains elements in sorted order (either always increasing or always decreasing). Used for "next greater/smaller element" problems.

Already covered in Chapter 12.5 — Next Greater Element.

### 45.2 Monotonic Queue (Sliding Window Maximum)

```python
from collections import deque

def max_sliding_window(nums, k):
    """Find the maximum in each window of size k."""
    dq = deque()  # stores indices, front is always the max
    result = []

    for i in range(len(nums)):
        while dq and dq[0] < i - k + 1:
            dq.popleft()    # remove indices outside the window
        while dq and nums[dq[-1]] <= nums[i]:
            dq.pop()        # remove smaller elements
        dq.append(i)
        if i >= k - 1:
            result.append(nums[dq[0]])

    return result
```

---

## 46. LRU Cache

### 46.1 What is an LRU Cache?

Least Recently Used (LRU) Cache evicts the least recently accessed item when the cache is full. Supports O(1) get and put.

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = OrderedDict()

    def get(self, key):
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)  # mark as recently used
        return self.cache[key]

    def put(self, key, value):
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)  # remove oldest
```

---

## 47. Bloom Filters

### 47.1 What is a Bloom Filter?

A space-efficient probabilistic data structure that tests whether an element is in a set. It can have **false positives** (says "maybe yes" when it's actually no) but **never false negatives** (if it says "no", it's definitely no).

Used in: spell checkers, web crawlers (avoid revisiting URLs), databases (skip disk reads for non-existent keys).

---

# PART 7: PROBLEM-SOLVING PATTERNS

---

## 48. Pattern: Frequency Counter

**When to use:** Problems involving counting occurrences, comparing frequencies, finding duplicates or anagrams.

```python
from collections import Counter

def is_anagram(s, t):
    return Counter(s) == Counter(t)

def top_k_frequent(nums, k):
    count = Counter(nums)
    return [x for x, _ in count.most_common(k)]

def first_unique_char(s):
    count = Counter(s)
    for i, c in enumerate(s):
        if count[c] == 1:
            return i
    return -1
```

---

## 49. Pattern: Fast and Slow Pointers

**When to use:** Linked list cycles, finding the middle, and certain array problems.

```python
def find_cycle_start(head):
    """Find where the cycle begins in a linked list."""
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    else:
        return None  # no cycle

    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    return slow  # cycle start
```

---

## 50. Pattern: Merge Intervals

**When to use:** Overlapping intervals, scheduling conflicts, finding gaps.

```python
def merge_intervals(intervals):
    intervals.sort(key=lambda x: x[0])
    merged = [intervals[0]]

    for start, end in intervals[1:]:
        if start <= merged[-1][1]:
            merged[-1][1] = max(merged[-1][1], end)
        else:
            merged.append([start, end])

    return merged

# merge_intervals([[1,3],[2,6],[8,10],[15,18]])
# → [[1,6],[8,10],[15,18]]
```

---

## 51. Pattern: Cyclic Sort

**When to use:** Problems involving arrays with numbers in the range [1, n] or [0, n].

```python
def find_missing_number(nums):
    """Find the missing number in [0, n]."""
    n = len(nums)
    # Cyclic sort
    i = 0
    while i < n:
        correct = nums[i]
        if correct < n and nums[i] != nums[correct]:
            nums[i], nums[correct] = nums[correct], nums[i]
        else:
            i += 1

    for i in range(n):
        if nums[i] != i:
            return i
    return n
```

---

## 52. Pattern: Top-K Elements

**When to use:** Finding K largest, smallest, most frequent, or closest elements.

```python
import heapq

def k_closest_points(points, k):
    """Find k closest points to origin."""
    heap = []
    for x, y in points:
        dist = x*x + y*y
        heapq.heappush(heap, (-dist, [x, y]))
        if len(heap) > k:
            heapq.heappop(heap)
    return [point for _, point in heap]
```

---

## 53. Pattern: Modified Binary Search

**When to use:** Sorted or rotated arrays, finding boundaries.

```python
def search_rotated(nums, target):
    """Search in a rotated sorted array."""
    left, right = 0, len(nums) - 1

    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid

        if nums[left] <= nums[mid]:   # left half is sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:                          # right half is sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1

    return -1

def find_peak_element(nums):
    """Find any peak (element greater than its neighbors)."""
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] < nums[mid + 1]:
            left = mid + 1
        else:
            right = mid
    return left
```

---

## 54. Pattern: Subsets / Combinations / Permutations

Already covered in Chapter 40 (Backtracking). Quick summary:

```python
# Subsets: include or exclude each element → 2ⁿ subsets
# Combinations: choose k from n → C(n,k)
# Permutations: arrange all elements → n!

def combinations(nums, k):
    result = []
    def backtrack(start, current):
        if len(current) == k:
            result.append(current[:])
            return
        for i in range(start, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()
    backtrack(0, [])
    return result
```

---

## 55. Pattern: Matrix Traversal

**When to use:** Grid-based problems — islands, paths, flood fill.

```python
# Four directions
directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]

# Eight directions (including diagonals)
directions_8 = [(0,1),(0,-1),(1,0),(-1,0),(1,1),(1,-1),(-1,1),(-1,-1)]

def flood_fill(image, sr, sc, new_color):
    original = image[sr][sc]
    if original == new_color:
        return image

    rows, cols = len(image), len(image[0])

    def dfs(r, c):
        if r < 0 or r >= rows or c < 0 or c >= cols:
            return
        if image[r][c] != original:
            return
        image[r][c] = new_color
        for dr, dc in directions:
            dfs(r + dr, c + dc)

    dfs(sr, sc)
    return image
```

---

## 56. Pattern: Knapsack Variations

### 56.1 0/1 Knapsack (Each Item Once)

Covered in Chapter 39.1.

### 56.2 Unbounded Knapsack (Unlimited Items)

```python
def unbounded_knapsack(weights, values, capacity):
    dp = [0] * (capacity + 1)
    for w in range(1, capacity + 1):
        for i in range(len(weights)):
            if weights[i] <= w:
                dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[capacity]
```

### 56.3 Subset Sum

```python
def can_partition(nums):
    """Can you partition into two subsets with equal sum?"""
    total = sum(nums)
    if total % 2:
        return False
    target = total // 2
    dp = [False] * (target + 1)
    dp[0] = True
    for num in nums:
        for j in range(target, num - 1, -1):
            dp[j] = dp[j] or dp[j - num]
    return dp[target]
```

---

# PART 8: INTERVIEW PREPARATION

---

## 57. How to Solve Problems in Interviews

### 57.1 The Framework

1. **Clarify** (1-2 min): Ask about edge cases, constraints, input size
2. **Think out loud** (2-3 min): Discuss your approach. Mention brute force first, then optimize
3. **Plan** (1-2 min): Outline the algorithm before coding
4. **Code** (10-15 min): Write clean, working code
5. **Test** (2-3 min): Walk through examples, check edge cases
6. **Analyze** (1 min): State time and space complexity

### 57.2 Tips

- **Start with brute force** — it shows you understand the problem, then optimize
- **Think about which data structure** would make the key operation faster
- **Identify the pattern** — most problems are variations of known patterns
- **Communicate constantly** — interviewers want to see your thought process
- **Handle edge cases:** empty input, single element, duplicates, negative numbers, very large input

---

## 58. The Most Important 50 Problems (by Category)

### Arrays & Hashing
1. Two Sum
2. Best Time to Buy and Sell Stock
3. Contains Duplicate
4. Product of Array Except Self
5. Maximum Subarray (Kadane's)
6. Group Anagrams
7. Top K Frequent Elements

### Two Pointers
8. Valid Palindrome
9. Three Sum
10. Container With Most Water
11. Trapping Rain Water

### Sliding Window
12. Longest Substring Without Repeating Characters
13. Minimum Window Substring
14. Maximum Sum Subarray of Size K

### Stack
15. Valid Parentheses
16. Min Stack
17. Daily Temperatures
18. Largest Rectangle in Histogram

### Binary Search
19. Search in Rotated Sorted Array
20. Find Minimum in Rotated Sorted Array
21. Koko Eating Bananas
22. Median of Two Sorted Arrays

### Linked List
23. Reverse Linked List
24. Merge Two Sorted Lists
25. Linked List Cycle
26. LRU Cache

### Trees
27. Maximum Depth of Binary Tree
28. Invert Binary Tree
29. Validate BST
30. Lowest Common Ancestor
31. Binary Tree Level Order Traversal
32. Serialize and Deserialize Binary Tree

### Heap / Priority Queue
33. Kth Largest Element
34. Merge K Sorted Lists
35. Find Median from Data Stream

### Graphs
36. Number of Islands
37. Clone Graph
38. Course Schedule (Topological Sort)
39. Word Ladder (BFS)
40. Network Delay Time (Dijkstra)

### Dynamic Programming
41. Climbing Stairs
42. Coin Change
43. Longest Increasing Subsequence
44. Longest Common Subsequence
45. Word Break
46. House Robber
47. 0/1 Knapsack
48. Edit Distance

### Backtracking
49. Subsets / Permutations / Combinations
50. N-Queens

---

## 59. Common Mistakes and How to Avoid Them

**Off-by-one errors:** Double-check your loop bounds. Use `< len(arr)` not `<= len(arr)`.

**Not handling edge cases:** Always check: empty input, single element, all same elements, negative numbers.

**Modifying a collection while iterating:** Create a copy or use indices.

**Forgetting to mark visited in graph/tree problems:** Causes infinite loops.

**Using the wrong data structure:** If you need fast lookup, use a set/dict (O(1)), not a list (O(n)).

**Not considering integer overflow:** Less of an issue in Python (arbitrary precision), but important in other languages.

**Mixing up `=` and `==`:** Assignment vs comparison.

**Forgetting `return` statements:** Especially in recursive functions.

---

## 60. Where to Go Next

### 60.1 Practice Platforms

- **LeetCode** (leetcode.com): The gold standard. 2000+ problems. Study the "Top Interview 150" list.
- **NeetCode** (neetcode.io): Curated 150 problems organized by pattern, with video explanations.
- **HackerRank** (hackerrank.com): Good for beginners. Structured learning paths.
- **Codeforces** (codeforces.com): Competitive programming. Harder problems.

### 60.2 Recommended Study Plan

**Weeks 1-2:** Arrays, Strings, Hash Maps, Two Pointers — solve 20-30 easy problems.

**Weeks 3-4:** Stacks, Queues, Linked Lists, Sliding Window — solve 20 easy/medium problems.

**Weeks 5-6:** Trees, BSTs, BFS, DFS — solve 20 medium problems.

**Weeks 7-8:** Graphs, Heaps, Greedy — solve 20 medium problems.

**Weeks 9-10:** Dynamic Programming, Backtracking — solve 20 medium/hard problems.

**Weeks 11-12:** Binary Search (advanced), Tries, Union-Find, Bit Manipulation — solve 15 medium/hard problems.

**Ongoing:** Mock interviews. Time yourself. Explain your solution out loud.

### 60.3 The Golden Rules

- **Consistency > Intensity:** 2 problems per day for 3 months beats 20 problems in one weekend.
- **Understand, don't memorize:** If you understand the pattern, you can solve variations.
- **Review your mistakes:** Keep a notebook of problems you got wrong and why.
- **Time yourself:** In an interview, you have 30-45 minutes. Practice under time pressure.
- **Explain out loud:** The ability to communicate your thinking is as important as the code.

---

*This guide covers every major Data Structure and Algorithm concept with clear explanations and working Python code. For continued practice, use LeetCode with the NeetCode roadmap (neetcode.io/roadmap) as a guide.*
