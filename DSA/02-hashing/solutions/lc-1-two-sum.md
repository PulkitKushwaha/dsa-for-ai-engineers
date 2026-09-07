# LC 1 - Two Sum

## Problem Link

[Open LC 1 - Two Sum on LeetCode](https://leetcodeblem Statement

Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that:

```python
nums[i] + nums[j] == target
```

You may assume:

- Exactly one valid answer exists.
- You may not use the same element twice.

Return the indices in any order.

---

# Examples

### Example 1

```python
nums = [2,7,11,15]
target = 9
```

Output:

```python
[0,1]
```

Explanation:

```python
2 + 7 = 9
```

---

### Example 2

```python
nums = [3,2,4]
target = 6
```

Output:

```python
[1,2]
```

Explanation:

```python
2 + 4 = 6
```

---

### Example 3

```python
nums = [3,3]
target = 6
```

Output:

```python
[0,1]
```

Explanation:

```python
3 + 3 = 6
```

---

# Pattern Recognition

## Signals

Look for:

```text
Pair
Target Sum
Two Numbers
Required Difference
Return Indices
```

---

## Pattern

```text
Hashing -> Complement Lookup
```

---

## Recognition Shortcut

Ask:

```text
What value am I missing?
```

---

Example:

```python
target = 10
current = 6
```

Need:

```python
4
```

Question:

```text
Have I already seen 4?
```

If yes:

Answer found.

---

# Brute Force Approach

## Idea

Check every pair.

For each element:

Compare it against every remaining element.

---

## Solution

```python
class Solution:
    def twoSum(self, nums, target):

        n = len(nums)

        for i in range(n):

            for j in range(i + 1, n):

                if nums[i] + nums[j] == target:
                    return [i, j]
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

We repeatedly search for a valid partner.

Example:

```python
nums = [2,7,11,15]
```

For:

```python
2
```

we search for:

```python
7
```

Then later we search again and again.

Repeated work.

---

# Key Observation

Instead of asking:

```text
Can I find the partner later?
```

Ask:

```text
Have I already seen the partner?
```

---

Example

```python
target = 9
num = 7
```

Needed:

```python
needed = target - num
needed = 2
```

Question:

```text
Have I already seen 2?
```

If yes:

Answer found.

---

# Optimal Approach

## Data Structure

```python
dict()
```

Store:

```text
number -> index
```

Example:

```python
{
    2:0,
    7:1,
    11:2
}
```

---

## Why Not Store

```text
index -> number
```

Because we want fast lookup of:

```python
needed
```

Example:

```python
if needed in lookup
```

which is:

```text
O(1)
```

---

## Algorithm

For each number:

1. Calculate the complement.

```python
needed = target - num
```

2. Check whether complement already exists.

```python
if needed in lookup
```

3. If yes:

Return answer.

4. Otherwise:

Store current number and index.

---

# Optimal Solution

```python
class Solution:
    def twoSum(self, nums, target):

        lookup = {}

        for i, num in enumerate(nums):

            needed = target - num

            if needed in lookup:
                return [lookup[needed], i]

            lookup[num] = i
```

---

# Important Interview Question

Why do we check:

```python
if needed in lookup
```

before:

```python
lookup[num] = i
```

?

Consider:

```python
nums = [3,3]
target = 6
```

At index:

```python
0
```

We should not immediately match the element with itself.

Therefore:

1. First search for a previous partner.
2. Then store the current element.

This ensures:

```text
Different indices are used.
```

---

# Dry Run

Input:

```python
nums = [2,7,11,15]
target = 9
```

---

Initially

```python
lookup = {}
```

---

### Index 0

```python
num = 2
needed = 7
```

Exists?

```python
No
```

Store:

```python
{
    2:0
}
```

---

### Index 1

```python
num = 7
needed = 2
```

Exists?

```python
Yes
```

Answer:

```python
[0,1]
```

---

# Another Dry Run

Input:

```python
nums = [3,2,4]
target = 6
```

---

### Index 0

```python
num = 3
needed = 3
```

Not found.

Store:

```python
{
    3:0
}
```

---

### Index 1

```python
num = 2
needed = 4
```

Not found.

Store:

```python
{
    3:0,
    2:1
}
```

---

### Index 2

```python
num = 4
needed = 2
```

Found.

Return:

```python
[1,2]
```

---

# Complexity Analysis

## Time Complexity

Single traversal:

```text
O(n)
```

Each lookup:

```text
O(1)
```

average case.

---

## Space Complexity

HashMap stores numbers:

```text
O(n)
```

---

# Why Set Is Not Enough

Set only stores:

```text
values
```

Example:

```python
{2,7,11}
```

---

Need:

```text
Return indices
```

Therefore:

We need:

```text
number -> index
```

which requires:

```python
dict()
```

---

# Common Mistakes

## Mistake 1

Using a set.

```python
seen = set()
```

A set cannot return:

```text
Index
```

---

## Mistake 2

Storing

```text
index -> number
```

instead of:

```text
number -> index
```

This destroys fast complement lookup.

---

## Mistake 3

Searching after storing.

Wrong order can accidentally allow matching an element with itself.

Always:

```python
Check first
Store later
```

---

## Mistake 4

Thinking this is a Frequency problem.

It is not.

This is:

```text
Complement Lookup
```

---

# Reusable Template

## Complement Lookup Template

```python
lookup = {}

for index, value in enumerate(items):

    needed = target - value

    if needed in lookup:
        return [lookup[needed], index]

    lookup[value] = index
```

---

# Pattern Comparison

## LC 217

Question:

```text
Have I seen this before?
```

Structure:

```python
set
```

---

## LC 242

Question:

```text
How many times does this appear?
```

Structure:

```python
dict / Counter
```

---

## LC 383

Question:

```text
Do I have enough inventory?
```

Structure:

```python
dict / Counter
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

# Similar Problems

Easy:

- LC 170 - Two Sum III

Medium:

- LC 560 - Subarray Sum Equals K
- LC 167 - Two Sum II (Two Pointers)
- LC 15 - 3Sum
- LC 18 - 4Sum

---

# What to Memorize

Do NOT memorize the code.

Memorize:

```text
Signal:
Pair
Target
Sum

↓

Question:
What value am I missing?

↓

Formula:
needed = target - num

↓

Question:
Have I already seen needed?

↓

Structure:
Dictionary

↓

Mapping:
number -> index

↓

Pattern:
Complement Lookup
```

---

# Pattern Takeaway

LC 1 introduces one of the most important DSA interview patterns:

```text
Complement Lookup
```

Instead of searching forward for a partner:

```text
Store previous information.
Ask whether the required partner already exists.
```

This transforms:

```text
O(n²)
```

into:

```text
O(n)
```

and is the foundation for many future HashMap and Prefix Sum problems.
`