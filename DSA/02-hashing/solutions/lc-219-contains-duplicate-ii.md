# LC 219 - Contains Duplicate II

## Problem Link

https://leetcode.com/problems/contains-duplicate-ii/

---

# Problem Statement

Given an integer array `nums` and an integer `k`, return:

```python
True
```

if there are two distinct indices `i` and `j` in the array such that:

```python
nums[i] == nums[j]
```

and

```python
abs(i - j) <= k
```

Otherwise return:

```python
False
```

---

# Examples

### Example 1

```python
nums = [1,2,3,1]
k = 3
```

Output:

```python
True
```

Explanation:

```python
nums[0] = nums[3] = 1
```

Distance:

```python
3 - 0 = 3
```

Since:

```python
3 <= 3
```

return:

```python
True
```

---

### Example 2

```python
nums = [1,0,1,1]
k = 1
```

Output:

```python
True
```

Explanation:

```python
nums[2] = nums[3] = 1
```

Distance:

```python
1
```

---

### Example 3

```python
nums = [1,2,3,1,2,3]
k = 2
```

Output:

```python
False
```

Duplicates exist, but none occur within distance `2`.

---

# Pattern Recognition

## Signals

Look for:

```text
Duplicate
Same value
Nearby
Within k
Distance
Index difference
```

---

## Pattern

```text
Hashing -> Duplicate Detection + Index Tracking
```

---

## Recognition Shortcut

Ask:

```text
Where did I last see this value?
```

If the previous occurrence exists:

```text
How far away was it?
```

---

# Relationship with LC 217

### LC 217

Question:

```text
Have I seen this value before?
```

Data Structure:

```python
set()
```

---

### LC 219

Question:

```text
Have I seen this value before?
AND
How far away was it?
```

Data Structure:

```python
dict()
```

Because a set cannot store the previous index.

---

# Brute Force Approach

## Idea

Check every pair.

For every pair:

```python
nums[i] == nums[j]
```

and

```python
abs(i - j) <= k
```

---

## Solution

```python
class Solution:
    def containsNearbyDuplicate(self, nums, k):

        n = len(nums)

        for i in range(n):

            for j in range(i + 1, n):

                if nums[i] == numsif abs(i - j) <= k:
                        return True

        return False
```

---

## Complexity Analysis

### Time Complexity

```text
O(n²)
```

Nested loops.

---

### Space Complexity

```text
O(1)
```

---

## Why It Is Not Optimal

We repeatedly search through all previous elements.

Most of that work is unnecessary.

---

# Key Observation

We do not need all previous positions.

We only need:

```text
The most recent index
```

for each value.

---

# Data Structure

Use a dictionary.

Store:

```text
value -> last index
```

Example:

```python
{
    1:0,
    2:1,
    3:2
}
```

---

# Algorithm

For each value:

1. Check whether it already exists in the dictionary.

2. If yes:

Calculate:

```python
current_index - previous_index
```

3. If:

```python
distance <= k
```

return:

```python
True
```

4. Update the dictionary using the latest index.

---

# Optimal Solution

```python
class Solution:
    def containsNearbyDuplicate(self, nums, k):

        lookup = {}

        for i, num in enumerate(nums):

            if num in lookup:

                if i - lookup[num] <= k:
                    return True

            lookup[num] = i

        return False
```

---

# Dry Run

Input:

```python
nums = [1,2,3,1]
k = 3
```

---

Initially:

```python
lookup = {}
```

---

### Index 0

```python
num = 1
```

Store:

```python
{
    1:0
}
```

---

### Index 1

```python
num = 2
```

Store:

```python
{
    1:0,
    2:1
}
```

---

### Index 2

```python
num = 3
```

Store:

```python
{
    1:0,
    2:1,
    3:2
}
```

---

### Index 3

```python
num = 1
```

Already exists.

Previous index:

```python
0
```

Distance:

```python
3 - 0 = 3
```

Check:

```python
3 <= 3
```

True.

Return:

```python
True
```

---

# Why Do We Update the Index?

Consider:

```python
nums = [1,0,1,1]
```

Suppose:

```python
lookup[1] = 0
```

When we encounter the next:

```python
1
```

at index:

```python
2
```

we should update:

```python
lookup[1] = 2
```

because the most recent occurrence is more useful for future distance checks.

---

# Complexity Analysis

## Time Complexity

Single traversal:

```text
O(n)
```

Dictionary lookup:

```text
O(1)
```

average case.

---

## Space Complexity

Dictionary stores seen values:

```text
O(n)
```

---

# Why Set Is Not Enough

A set can only answer:

```text
Have I seen this value?
```

It cannot answer:

```text
Where did I see it?
```

Example:

```python
seen = {1,2,3}
```

No index information exists.

This problem requires:

```text
value -> last index
```

Therefore:

```python
dict()
```

is needed.

---

# Common Mistakes

## Mistake 1

Using a set.

```python
seen = set()
```

You lose index information.

---

## Mistake 2

Storing every occurrence in a list.

Not necessary.

We only need:

```text
Most recent index
```

---

## Mistake 3

Using:

```python
abs(i - lookup[num])
```

Not incorrect, but unnecessary.

Since:

```python
lookup[num]
```

always comes from an earlier index:

```python
i - lookup[num]
```

is already positive.

---

## Mistake 4

Forgetting to update:

```python
lookup[num] = i
```

The latest occurrence should replace the older one.

---

# Reusable Template

## Last Occurrence Template

```python
lookup = {}

for index, value in enumerate(items):

    if value in lookup:

        if index - lookup[value] <= limit:
            return True

    lookup[value] = index

return False
```

---

# Pattern Comparison

## LC 217

Question:

```text
Have I seen this value before?
```

Structure:

```python
set
```

---

## LC 1

Question:

```text
What value am I missing?
```

Structure:

```python
dict
```

Mapping:

```text
number -> index
```

---

## LC 219

Question:

```text
Where was this value last seen?
```

Structure:

```python
dict
```

Mapping:

```text
value -> last index
```

---

# Similar Problems

Easy:

- LC 217 - Contains Duplicate

Medium:

- LC 220 - Contains Duplicate III
- Sliding Window variants
- Longest Substring Without Repeating Characters (later)

---

# What to Memorize

Do NOT memorize the code.

Memorize:

```text
Signal:
Duplicate
Nearby
Distance
Within k

↓

Question:
Where did I last see this value?

↓

Structure:
Dictionary

↓

Mapping:
value -> last index

↓

Check:
current_index