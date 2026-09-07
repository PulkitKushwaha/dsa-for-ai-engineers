# Python DSA Toolbox

> Before solving DSA problems, you must know the tools available to you.
>
> This file covers the most important Python data structures, utilities, and complexity concepts required for coding interviews.
>
> Do not memorize everything.
>
> Focus on understanding:
>
> 1. What each structure is good at.
> 2. Its time complexity.
> 3. Which DSA patterns commonly use it.

---

# The Golden Rule

When solving DSA questions, always ask:

"What operations will I perform most often?"

Examples:

Need fast lookup?
→ Dictionary / Set

Need add/remove at both ends?
→ Deque

Need top K?
→ Heap

Need count frequencies?
→ Counter

The fastest solution usually comes from selecting the correct data structure.

---

# Time Complexity Cheat Sheet

## The Most Important Complexity Values

| Complexity | Name | Good? |
|------------|--------|---------|
| O(1) | Constant | Excellent |
| O(log n) | Logarithmic | Excellent |
| O(n) | Linear | Good |
| O(n log n) | Linearithmic | Acceptable |
| O(n²) | Quadratic | Usually Bad |
| O(2ⁿ) | Exponential | Dangerous |

---

# Common Interview Benchmark

For:

n = 100,000

Approximate limits:

✅ O(n)

✅ O(n log n)

⚠️ O(n²)

❌ O(2ⁿ)

---

# Python List

Most commonly used structure in DSA.

Under the hood:

```python
arr = []
```

Dynamic array.

---

## Common Operations

```python
arr.append(x)
```

Add at end.

---

```python
arr.pop()
```

Remove from end.

---

```python
arr[i]
```

Access by index.

---

## Complexity

| Operation | Complexity |
|------------|------------|
| Access | O(1) |
| Append | O(1) |
| Pop End | O(1) |
| Insert Beginning | O(n) |
| Delete Beginning | O(n) |

---

## Used In

- Arrays
- Sliding Window
- Binary Search
- Dynamic Programming

---

# Dictionary (Hash Map)

Most powerful structure in interviews.

---

## Create

```python
d = {}
```

---

## Insert

```python
d[key] = value
```

---

## Lookup

```python
if key in d:
```

---

## Complexity

| Operation | Complexity |
|------------|------------|
| Insert | O(1) |
| Lookup | O(1) |
| Delete | O(1) |

Average case.

---

## Used In

- Hashing
- Caching
- Frequency counting
- Graphs

---

## Example

```python
nums = [1,2,2,3]

freq = {}

for n in nums:
    freq[n] = freq.get(n,0) + 1
```

Result:

```python
{
  1:1,
  2:2,
  3:1
}
```

---

# Set

Stores unique values.

---

## Create

```python
s = set()
```

---

## Add

```python
s.add(x)
```

---

## Check

```python
if x in s:
```

---

## Complexity

| Operation | Complexity |
|------------|------------|
| Add | O(1) |
| Lookup | O(1) |
| Delete | O(1) |

---

## Used In

- Duplicate detection
- Visited nodes
- Graph traversal

---

## Example

```python
nums = [1,2,2,3]

seen = set()

for n in nums:
    if n in seen:
        return True

    seen.add(n)
```

---

# Counter

Frequency counting made easy.

---

## Import

```python
from collections import Counter
```

---

## Example

```python
from collections import Counter

nums = [1,1,2,2,2]

counter = Counter(nums)
```

Result:

```python
{
  1:2,
  2:3
}
```

---

## Useful Operations

```python
counter.most_common(3)
```

Returns top 3 frequencies.

---

## Used In

- Anagrams
- Frequency counting
- Top K problems

---

# defaultdict

Dictionary with automatic default values.

---

## Import

```python
from collections import defaultdict
```

---

## Example

Without defaultdict:

```python
d = {}

if key not in d:
    d[key] = []

d[key].append(value)
```

---

With defaultdict:

```python
d = defaultdict(list)

d[key].append(value)
```

Cleaner.

---

## Most Common Types

### List

```python
defaultdict(list)
```

---

### Integer Counter

```python
defaultdict(int)
```

---

### Set

```python
defaultdict(set)
```

---

## Used In

- Graphs
- Group Anagrams
- Adjacency Lists

---

# Deque

Double-ended queue.

---

## Import

```python
from collections import deque
```

---

## Create

```python
q = deque()
```

---

## Add

```python
q.append(x)
```

Right side.

```python
q.appendleft(x)
```

Left side.

---

## Remove

```python
q.pop()
```

Right side.

```python
q.popleft()
```

Left side.

---

## Complexity

All operations:

```python
O(1)
```

---

## Why Not Use List?

Bad:

```python
arr.pop(0)
```

Complexity:

```python
O(n)
```

---

Good:

```python
deque.popleft()
```

Complexity:

```python
O(1)
```

---

## Used In

- BFS
- Queues
- Sliding Window

---

# Heap (Priority Queue)

Used when you need:

- Top K
- Maximum
- Minimum

efficiently.

---

## Import

```python
import heapq
```

---

# Min Heap

```python
heap = []
```

Insert:

```python
heapq.heappush(heap, x)
```

Remove smallest:

```python
heapq.heappop(heap)
```

---

## Example

```python
heap = []

heapq.heappush(heap, 10)
heapq.heappush(heap, 5)
heapq.heappush(heap, 20)

print(heapq.heappop(heap))
```

Output:

```python
5
```

---

# Max Heap

Python only supports min heap.

Common trick:

```python
heapq.heappush(heap, -value)
```

---

Example

```python
heapq.heappush(heap, -10)
heapq.heappush(heap, -20)

largest = -heapq.heappop(heap)
```

---

## Complexity

| Operation | Complexity |
|------------|------------|
| Insert | O(log n) |
| Remove | O(log n) |
| Peek | O(1) |

---

## Used In

- Top K Frequent Elements
- Kth Largest
- Merge K Sorted Lists

---

# Sorting

Extremely common in interviews.

---

## Ascending

```python
nums.sort()
```

---

## Descending

```python
nums.sort(reverse=True)
```

---

## Sorted Copy

```python
result = sorted(nums)
```

---

## Sort By Key

```python
words.sort(key=len)
```

---

## Complexity

```python
O(n log n)
```

---

# Enumerate

Access index and value together.

---

Instead of:

```python
for i in range(len(nums)):
```

Use:

```python
for i, num in enumerate(nums):
```

---

Example

```python
for i, n in enumerate(nums):
    print(i, n)
```

---

# Zip

Traverse multiple arrays.

---

Example

```python
a = [1,2,3]
b = [4,5,6]

for x, y in zip(a,b):
    print(x,y)
```

---

# List Comprehension

Common interview shorthand.

---

Traditional

```python
squares = []

for i in range(5):
    squares.append(i*i)
```

---

Pythonic

```python
squares = [i*i for i in range(5)]
```

---

# Useful Interview Tricks

## Reverse List

```python
nums[::-1]
```

---

## Reverse String

```python
s[::-1]
```

---

## Membership Check

```python
if x in my_set:
```

O(1)

---

## Get With Default

```python
d.get(key, 0)
```

avoids:

```python
if key not in d
```

---

# Mapping Patterns To Python Structures

| Pattern | Main Structure |
|----------|---------------|
| Hashing | dict / set |
| Frequency Count | Counter |
| Graph | defaultdict(list) |
| BFS | deque |
| DFS | recursion / stack |
| Sliding Window | list + dict |
| Heap Problems | heapq |
| Binary Search | list |
| Dynamic Programming | list / dict |
| Backtracking | recursion |

---

# Must Memorize

If you only memorize one section from this file, memorize this:

| Need | Use |
|--------|------|
| Fast Lookup | dict |
| Unique Values | set |
| Frequency Count | Counter |
| Queue | deque |
| Top K | heapq |
| Graph | defaultdict |
| Ordered Data | list |

These six structures solve a huge percentage of LeetCode problems.

---

# Final Takeaway

Most DSA interview questions are not testing syntax.

They are testing whether you can recognize:

1. Which pattern applies.
2. Which data structure supports that pattern.
3. Which operations can be performed efficiently.

Mastering this toolbox dramatically reduces the gap between recognizing a solution and implementing it in Python.