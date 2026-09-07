# LC 128 - Longest Consecutive Sequence

## Problem Link

[Open LC 128 - Longest Consecutive Sequence on LeetCode](https://leetcode.com/problems/longest-consecutive-sequence/)

---

## Problem Statement

Given an unsorted array of integers `nums`, return the length of the longest sequence of consecutive values.

The required algorithm should run in `O(n)` time.

---

## Examples

### Example 1

```python
nums = [100, 4, 200, 1, 3, 2]
```

Output:

```python
4
```

Explanation:

The longest consecutive sequence is:

```text
1 -> 2 -> 3 -> 4
```

Its length is `4`.

### Example 2

```python
nums = [0, 3, 7, 2, 5, 8, 4, 6, 0, 1]
```

Output:

```python
9
```

Explanation:

The longest consecutive sequence is:

```text
0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8
```

Its length is `9`.

### Example 3

```python
nums = []
```

Output:

```python
0
```

---

# Pattern Recognition

## Signals

Look for:

```text
Longest consecutive sequence
Continuous integer range
Unsorted input
Check whether the next number exists
Linear-time requirement
```

## Pattern

```text
Hashing -> Fast Existence Lookup -> Sequence Expansion
```

## Recognition Shortcut

Ask two questions:

```text
1. Can I check whether the next number exists in O(1) average time?
2. Can I avoid expanding the same sequence multiple times?
```

The answers lead to:

```text
Convert the input to a set.
Expand only from the start of a sequence.
```

## Mental Model

Imagine that every sequence is a chain.

```text
1 -> 2 -> 3 -> 4
```

Do not begin walking from every link. Begin only from the first link.

A number is the first element of a sequence when its predecessor does not exist:

```python
num - 1 not in numbers
```

---

# Brute Force Approach

## Idea

For every number, repeatedly search the original list for the next value.

```python
num + 1
num + 2
num + 3
```

## Example

Starting from `1`:

```text
Does 2 exist?
Does 3 exist?
Does 4 exist?
Does 5 exist?
```

## Why It Is Inefficient

Membership checking in a list takes `O(n)` time in the worst case:

```python
value in nums
```

If this search is repeated for many values, the overall solution can become `O(n^2)`.

---

# Sorting Approach

## Idea

Sort the values and count adjacent consecutive numbers.

## Solution

```python
class Solution:
    def longestConsecutive(self, nums):
        if not nums:
            return 0

        nums.sort()
        longest = 1
        current_length = 1

        for index in range(1, len(nums)):
            if nums[index] == nums[index - 1]:
                continue

            if nums[index] == nums[index - 1] + 1:
                current_length += 1
            else:
                current_length = 1

            longest = max(longest, current_length)

        return longest
```

## Complexity Analysis

### Time Complexity

```text
O(n log n)
```

Sorting dominates the traversal.

### Space Complexity

The extra-space cost depends on the sorting implementation. Sorting also mutates the input list.

## Why It Does Not Meet the Optimal Requirement

The problem asks for an `O(n)` solution. Sorting requires `O(n log n)` time, so hashing is needed for the optimal approach.

---

# Optimal Approach: HashSet and Sequence Starts

## Key Observation 1: Use a Set

Convert the input values into a set:

```python
numbers = set(nums)
```

This provides average `O(1)` existence checks:

```python
next_number in numbers
```

It also removes duplicates, which do not increase the length of a consecutive sequence.

## Key Observation 2: Expand Only from Sequence Starts

Consider:

```text
1 -> 2 -> 3 -> 4
```

If we expand from every number, we repeat work:

```text
From 1: 1, 2, 3, 4
From 2: 2, 3, 4
From 3: 3, 4
From 4: 4
```

Instead, expand only when the previous value is absent:

```python
if num - 1 not in numbers:
```

For this sequence:

```text
1 is a start because 0 does not exist.
2 is not a start because 1 exists.
3 is not a start because 2 exists.
4 is not a start because 3 exists.
```

The sequence is therefore expanded only once.

---

# Algorithm

1. Convert `nums` into a set.
2. Initialize `longest` to `0`.
3. Iterate through each unique number.
4. Check whether `num - 1` is absent.
5. If absent, `num` is the beginning of a sequence.
6. Keep checking `num + 1`, `num + 2`, and so on.
7. Count the sequence length.
8. Update the longest length.
9. Return the longest length.

---

# Optimal Solution

```python
class Solution:
    def longestConsecutive(self, nums):
        numbers = set(nums)
        longest = 0

        for num in numbers:
            if num - 1 not in numbers:
                current = num
                current_length = 1

                while current + 1 in numbers:
                    current += 1
                    current_length += 1

                longest = max(longest, current_length)

        return longest
```

---

# Alternative Clean Version Using `num + length`

The current number does not need to be updated separately. The sequence length can also act as the offset from the starting number.

```python
class Solution:
    def longestConsecutive(self, nums):
        numbers = set(nums)
        longest = 0

        for num in numbers:
            if num - 1 not in numbers:
                length = 1

                while num + length in numbers:
                    length += 1

                longest = max(longest, length)

        return longest
```

## How `num + length` Works

Suppose:

```python
num = 1
length = 1
```

The checks are:

```text
1 + 1 = 2
1 + 2 = 3
1 + 3 = 4
1 + 4 = 5
```

If `2`, `3`, and `4` exist but `5` does not, the final length is `4`.

Both optimal versions implement the same algorithm.

---

# Dry Run

Input:

```python
nums = [100, 4, 200, 1, 3, 2]
```

Convert to a set:

```python
numbers = {100, 4, 200, 1, 3, 2}
```

## Number `100`

Check:

```python
99 not in numbers
```

Therefore, `100` starts a sequence.

```text
100 exists
101 does not exist
```

Length:

```python
1
```

## Number `4`

Check:

```python
3 in numbers
```

`4` is not a sequence start, so skip it.

## Number `200`

Check:

```python
199 not in numbers
```

`200` starts a sequence of length `1`.

## Number `1`

Check:

```python
0 not in numbers
```

`1` is a sequence start.

Expand:

```text
1 -> 2 -> 3 -> 4
```

Length:

```python
4
```

## Numbers `2` and `3`

Their predecessors exist, so they are skipped as sequence starts.

Final answer:

```python
4
```

---

# Why the Time Complexity Is O(n)

At first, the nested `while` loop may make the algorithm look quadratic.

However, sequence expansion begins only from a true starting number.

For the sequence:

```text
1 -> 2 -> 3 -> 4
```

The complete sequence is expanded only from `1`.

Numbers `2`, `3`, and `4` fail the start condition and do not expand the sequence again.

Across all sequences, each unique number participates in sequence expansion at most once.

Therefore:

```text
Set construction: O(n)
Outer traversal: O(n)
Total sequence expansion across all starts: O(n)
```

Overall:

```text
O(n)
```

---

# Complexity Analysis

Let `n` be the number of input values.

## Time Complexity

```text
O(n)
```

This uses average `O(1)` set membership checks.

## Space Complexity

```text
O(n)
```

The set may contain every distinct input value.

---

# Explaining the Fastest Submission

A fast LeetCode submission may look like this:

```python
class Solution:
    def longestConsecutive(self, nums):
        if not nums:
            return 0

        if len(nums) == 100000:
            return 2 if nums[0] == -100000000 else 100000

        res = 1

        for n in (nums := {*nums}):
            if n - 1 not in nums:
                length = 1

                while n + length in nums:
                    length += 1

                if length > res:
                    res = length

        return res
```

The core algorithm is valid, but part of this submission is a LeetCode-specific shortcut and should not be copied into interview or production code.

---

# Understanding `if not nums`

```python
if not nums:
    return 0
```

This checks whether the list is empty.

It is equivalent to:

```python
if len(nums) == 0:
    return 0
```

In the clean optimal solution, an explicit empty check is optional because:

```python
numbers = set([])
longest = 0
```

and the loop naturally returns `0`.

---

# Understanding `{*nums}`

```python
{*nums}
```

uses iterable unpacking inside a set literal.

It is equivalent to:

```python
set(nums)
```

Example:

```python
nums = [1, 2, 2, 3]
```

Both produce:

```python
{1, 2, 3}
```

For interviews, prefer:

```python
numbers = set(nums)
```

because it is clearer.

---

# Understanding the Walrus Operator `:=`

The expression:

```python
nums := {*nums}
```

assigns the set to `nums` and returns the assigned value within the same expression.

Therefore:

```python
for n in (nums := {*nums}):
```

is approximately equivalent to:

```python
nums = set(nums)

for n in nums:
```

The operator `:=` is called an assignment expression or walrus operator.

Although compact, the separate assignment is more readable for interviews and revision notes.

---

# Understanding `while n + length in nums`

Suppose:

```python
n = 1
length = 1
```

The condition checks:

```text
Is 2 present?
Is 3 present?
Is 4 present?
Is 5 present?
```

Each successful lookup increases the length.

```python
while n + length in nums:
    length += 1
```

This is a compact alternative to maintaining both `current` and `current_length`.

---

# Why the Hardcoded Length Check Is a Bad Practice

This section of the fast submission is not part of the algorithm:

```python
if len(nums) == 100000:
    return 2 if nums[0] == -100000000 else 100000
```

It assumes that inputs with a particular length and first value correspond to specific test cases.

Problems with this approach:

- It does not solve the general problem.
- Different valid inputs can have the same length.
- It relies on hidden test-case knowledge.
- It is unsuitable for interviews.
- It is unsuitable for production code.
- It can return incorrect results for unseen inputs.

Treat it as benchmark gaming, not algorithmic optimization.

---

# Clean Interview Version

Use this version in interviews and in the repository:

```python
class Solution:
    def longestConsecutive(self, nums: list[int]) -> int:
        numbers = set(nums)
        longest = 0

        for num in numbers:
            if num - 1 not in numbers:
                length = 1

                while num + length in numbers:
                    length += 1

                longest = max(longest, length)

        return longest
```

If the coding environment expects `List` from `typing`, use:

```python
from typing import List


class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numbers = set(nums)
        longest = 0

        for num in numbers:
            if num - 1 not in numbers:
                length = 1

                while num + length in numbers:
                    length += 1

                longest = max(longest, length)

        return longest
```

---

# Common Mistakes

## Mistake 1: Expanding from Every Number

Incorrect approach:

```python
for num in numbers:
    while num + 1 in numbers:
        ...
```

This repeats work for numbers in the same sequence.

Only expand when:

```python
num - 1 not in numbers
```

## Mistake 2: Using the Original List for Membership Checks

```python
while next_number in nums:
```

List membership is `O(n)`.

Use:

```python
while next_number in numbers:
```

where `numbers` is a set.

## Mistake 3: Sorting Despite the O(n) Requirement

Sorting produces a valid `O(n log n)` solution, but it does not meet the required linear-time target.

## Mistake 4: Counting Duplicates as Sequence Growth

Duplicates do not extend a consecutive sequence.

Converting to a set removes them naturally.

## Mistake 5: Starting with `longest = 1`

If the input is empty, initializing `longest` to `1` can return an incorrect result unless an explicit empty-input check exists.

Safer:

```python
longest = 0
```

## Mistake 6: Checking `num + 1` to Detect the Start

The start condition looks backward:

```python
num - 1 not in numbers
```

The expansion then looks forward:

```python
num + length in numbers
```

## Mistake 7: Copying Hardcoded LeetCode Test Hacks

Fast benchmark submissions are not automatically high-quality solutions. Learn the general algorithm rather than test-specific shortcuts.

---

# Interview Explanation

A concise explanation:

> I convert the array to a set so that I can check whether a value exists in average O(1) time. To avoid repeatedly traversing the same sequence, I only begin expansion from numbers whose predecessor is absent. From each sequence start, I count upward while the next consecutive value exists. Since every unique number is expanded at most once across all sequences, the total time complexity is O(n), with O(n) extra space.

---

# Reusable Sequence Expansion Template

```python
values = set(items)
best = 0

for value in values:
    if value - 1 not in values:
        length = 1

        while value + length in values:
            length += 1

        best = max(best, length)

return best
```

---

# Pattern Comparison

## LC 217 - Contains Duplicate

```text
Set purpose: Detect whether a value has appeared before.
```

## LC 202 - Happy Number

```text
Set purpose: Detect whether a generated state has repeated.
```

## LC 128 - Longest Consecutive Sequence

```text
Set purpose: Check whether neighboring values exist and expand a sequence.
```

The same structure supports different patterns depending on the question being asked.

---

# What to Memorize

Do not memorize the full code. Memorize this chain:

```text
Signal:
Longest consecutive sequence in unsorted data

Need:
Fast existence checks

Structure:
Set

Critical optimization:
Start only when num - 1 is absent

Expansion:
Keep checking num + 1, num + 2, and so on

Time:
O(n)

Space:
O(n)
```

---

# Pattern Takeaway

LC 128 teaches an advanced HashSet principle:

```text
Fast lookup alone is not enough.
Choose the correct starting points to avoid repeated work.
```

The decisive insight is:

```python
if num - 1 not in numbers:
```

This ensures each consecutive sequence is explored only from its beginning.

---

# Hashing Module Progress

- [x] LC 217 - Contains Duplicate
- [x] LC 242 - Valid Anagram
- [x] LC 387 - First Unique Character in a String
- [x] LC 383 - Ransom Note
- [x] LC 1 - Two Sum
- [x] LC 219 - Contains Duplicate II
- [x] LC 49 - Group Anagrams
- [x] LC 347 - Top K Frequent Elements
- [x] LC 202 - Happy Number
- [x] LC 128 - Longest Consecutive Sequence

```text
Hashing core problem set: Complete
Next roadmap topic: Two Pointers
```
