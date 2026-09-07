# LC 347 - Top K Frequent Elements

## Problem Link

https://leetcode.com/problems/top-k-frequent-elements/

---

# Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

You may return the answer in any order.

---

# Examples

### Example 1

```python
nums = [1,1,1,2,2,3]
k = 2
```

Output:

```python
[1,2]
```

Explanation:

```text
1 appears 3 times
2 appears 2 times
3 appears 1 time
```

Top 2 frequent elements:

```python
[1,2]
```

---

### Example 2

```python
nums = [1]
k = 1
```

Output:

```python
[1]
```

---

# Pattern Recognition

## Signals

Look for:

```text
Top K
Most Frequent
Most Common
Highest Frequency
K Most Repeated Elements
```

---

## Pattern

```text
Hashing -> Frequency Ranking
```

---

## Recognition Shortcut

Ask:

```text
How many times does every item occur?
```

Then:

```text
How do I rank items based on frequency?
```

---

# Key Observation

The problem is really two sub-problems:

### Step 1

Count frequencies.

```python
{
    1:3,
    2:2,
    3:1
}
```

### Step 2

Return the numbers with the highest frequencies.

---

# Brute Force Approach

## Idea

Count frequencies.

Then repeatedly search for the highest remaining frequency.

This requires multiple scans.

---

## Why It Is Inefficient

Repeated searching produces unnecessary work.

Instead, after building the frequency map, we can sort by frequency.

---

# Frequency Map

Input:

```python
nums = [1,1,1,2,2,3]
```

Build:

```python
{
    1:3,
    2:2,
    3:1
}
```

---

# Understanding `freq.items()`

Suppose:

```python
freq = {
    1:3,
    2:2,
    3:1
}
```

Then:

```python
freq.items()
```

returns:

```python
[
    (1,3),
    (2,2),
    (3,1)
]
```

Think of each tuple as:

```python
(number, frequency)
```

---

# Understanding Lambda

This:

```python
lambda x: x[1]
```

is equivalent to:

```python
def get_frequency(x):
    return x[1]
```

For:

```python
(1,3)
```

```python
x[0] -> 1
x[1] -> 3
```

Therefore:

```python
lambda x: x[1]
```

means:

```text
Sort using the frequency
```

instead of:

```text
Sort using the number
```

---

# Understanding `sorted()`

Example:

```python
sorted(
    freq.items(),
    key=lambda x: x[1],
    reverse=True
)
```

Python receives:

```python
[
    (1,3),
    (2,2),
    (3,1)
]
```

and sorts by:

```python
3
2
1
```

because:

```python
x[1]
```

represents frequency.

---

# Why `reverse=True`?

Without:

```python
reverse=True
```

Output:

```python
[
    (3,1),
    (2,2),
    (1,3)
]
```

Ascending frequency.

---

With:

```python
reverse=True
```

Output:

```python
[
    (1,3),
    (2,2),
    (3,1)
]
```

Descending frequency.

Exactly what we want.

---

# Sorting Solution

```python
class Solution:
    def topKFrequent(self, nums, k):

        freq = {}

        for num in nums:
            freq[num] = freq.get(num, 0) + 1

        sorted_items = sorted(
            freq.items(),
            key=lambda x: x[1],
            reverse=True
        )

        result = []

        for num, count in sorted_items[:k]:
            result.append(num)

        return result
```

---

# Dry Run

Input:

```python
nums = [4,4,4,5,5,6]
k = 1
```

---

Frequency Map

```python
{
    4:3,
    5:2,
    6:1
}
```

---

Items

```python
[
    (4,3),
    (5,2),
    (6,1)
]
```

---

Sort Descending

```python
[
    (4,3),
    (5,2),
    (6,1)
]
```

---

Take First K

```python
[
    (4,3)
]
```

Return:

```python
[4]
```

---

# Complexity Analysis

Let:

```text
n = total numbers
m = unique numbers
```

---

## Time Complexity

Build frequency map:

```text
O(n)
```

Sort unique values:

```text
O(m log m)
```

Total:

```text
O(n + m log m)
```

---

## Space Complexity

Frequency map:

```text
O(m)
```

---

# Common Mistakes

## Mistake 1

Returning:

```python
[(1,3),(2,2)]
```

instead of:

```python
[1,2]
```

We return numbers.

Not frequency pairs.

---

## Mistake 2

Sorting by key instead of frequency.

Wrong:

```python
sorted(freq.items())
```

This sorts by number.

---

Correct:

```python
sorted(
    freq.items(),
    key=lambda x: x[1]
)
```

Sort by frequency.

---

## Mistake 3

Forgetting:

```python
reverse=True
```

Without it:

```python
least frequent
```

appears first.

---

# Reusable Template

## Frequency Ranking Template

```python
freq = {}

for item in items:
    freq[item] = freq.get(item, 0) + 1

sorted_items = sorted(
    freq.items(),
    key=lambda x: x[1],
    reverse=True
)

return [
    item
    for item, count in sorted_items[:k]
]
```

---

# Pattern Comparison

## LC 242

Question:

```text
What is the frequency?
```

Structure:

```python
dict
```

---

## LC 49

Question:

```text
What group does it belong to?
```

Structure:

```python
dict
```

---

## LC 347

Question:

```text
Which items have the highest frequency?
```

Structure:

```python
dict
```

+

```text
ranking
```

---

# Interview Discussion

LeetCode's follow-up asks:

```text
Can you solve it better than O(n log n)?
```

Yes.

Using:

```text
HashMap + Heap
```

or

```text
HashMap + Bucket Sort
```

We will revisit this problem later during the Heap section.

For now, our goal is to understand the:

```text
Frequency Ranking Pattern
```

---

# What to Memorize

```text
Signal:
Top K
Most Frequent
Most Common

↓

Question:
How many times does each item appear?

↓

Build:
Frequency Map

↓

Rank:
Sort by Frequency

↓

Return:
Top K Items

↓

Pattern:
Frequency Ranking
```

---

# Pattern Takeaway

LC 347 teaches that HashMaps are not only for lookup.

They can also create:

```text
Frequency Rankings
```

which can then be sorted, heapified, or bucketed depending on the optimization required.

---