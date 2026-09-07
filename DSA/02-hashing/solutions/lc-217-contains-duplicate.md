# LC 217 - Contains Duplicate

## Problem Link

https://leetcode.com/problems/contains-duplicate/

---

# Problem Statement

Given an integer array `nums`, return:

```python
True
```

if any value appears at least twice in the array.

Otherwise return:

```python
False
```

---

# Examples

### Example 1

Input:

```python
nums = [1,2,3,1]
```

Output:

```python
True
```

Explanation:

```python
1
```

appears twice.

---

### Example 2

Input:

```python
nums = [1,2,3,4]
```

Output:

```python
False
```

All elements are unique.

---

# Pattern Recognition

## Signals

Question contains:

- Duplicate
- Repeated value
- Appears twice
- Seen before

---

## Pattern

```text
Hashing
```

Specifically:

```text
Duplicate Detection
```

---

## Recognition Shortcut

Whenever you see:

```text
Duplicate
Unique
Repeated
Seen Before
```

Immediately think:

```python
set()
```

---

# Brute Force Approach

## Idea

Compare every element with every other element.

---

## Code

```python
def containsDuplicate(nums):

    n = len(nums)

    for i in range(n):

        for j in range(i + 1, n):

            if nums[i] == numsreturn True

    return False
```

---

## Complexity

### Time

```python
O(n²)
```

### Space

```python
O(1)
```

---

## Why It Is Not Optimal

For every element we repeatedly search the rest of the array.

A lot of unnecessary work is performed.

---

# Optimal Approach

## Key Observation

Ask:

```text
Can I remember previously seen elements?
```

Answer:

```text
Yes
```

Instead of searching repeatedly, store elements as you encounter them.

---

## Data Structure

```python
set()
```

Reason:

```python
if x in my_set
```

Average complexity:

```python
O(1)
```

---

# Algorithm

1. Create an empty set.
2. Traverse the array.
3. If current number already exists in the set:
   - return True
4. Otherwise add it to the set.
5. If traversal completes:
   - return False

---

# Optimal Solution

```python
def containsDuplicate(nums):

    seen = set()

    for num in nums:

        if num in seen:
            return True

        seen.add(num)

    return False
```

---

# Dry Run

Input:

```python
[1,2,3,1]
```

---

Initially:

```python
seen = {}
```

Actually:

```python
set()
```

---

Read:

```python
1
```

Exists?

```python
No
```

Add:

```python
{1}
```

---

Read:

```python
2
```

Exists?

```python
No
```

Add:

```python
{1,2}
```

---

Read:

```python
3
```

Exists?

```python
No
```

Add:

```python
{1,2,3}
```

---

Read:

```python
1
```

Exists?

```python
Yes
```

Return:

```python
True
```

---

# Complexity Analysis

## Time Complexity

Single pass through array:

```python
O(n)
```

---

## Space Complexity

Set stores elements:

```python
O(n)
```

---

# Interview Discussion

## Alternative Approach

Sort the array first.

```python
nums.sort()
```

Then compare neighbouring values.

### Code

```python
def containsDuplicate(nums):

    nums.sort()

    for i in range(1, len(nums)):

        if nums[i] == nums[i - 1]:
            return True

    return False
```

---

## Complexity

Time:

```python
O(n log n)
```

Space:

```python
O(1)
```

(depending on sorting implementation)

---

## Tradeoff

Hash Set:

```python
Time  -> O(n)
Space -> O(n)
```

Sorting:

```python
Time  -> O(n log n)
Space -> O(1)
```

---

# Template Used

## Duplicate Detection Template

```python
seen = set()

for item in items:

    if item in seen:
        return True

    seen.add(item)

return False
```

---

# Common Mistakes

## Mistake 1

Using list instead of set.

Bad:

```python
seen = []

if num in seen
```

Complexity:

```python
O(n)
```

---

## Mistake 2

Not recognizing duplicate detection problems.

Keywords:

```text
Duplicate
Repeated
Unique
Seen Before
```

should immediately trigger:

```python
set()
```

---

# Similar Problems

## Easy

- LC 219 - Contains Duplicate II
- LC 217 - Contains Duplicate

## Medium

- LC 128 - Longest Consecutive Sequence
- LC 202 - Happy Number

---

# What To Memorize

Do NOT memorize the exact code.

Memorize:

```text
Signal:
Duplicate

↓

Question:
Have I seen this before?

↓

Data Structure:
Set

↓

Template:
Visited/Seen Set

↓

Complexity:
O(n)
```

---

# Pattern Takeaway

This problem teaches the most fundamental hashing idea:

> Use a Hash Set to remember previously seen values and eliminate unnecessary repeated searching.

Many future problems in Graphs, BFS, DFS, Sliding Window and advanced Hashing reuse this exact concept.