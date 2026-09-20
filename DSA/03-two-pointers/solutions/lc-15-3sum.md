# LC 15 - 3Sum

## Problem Link

[Open LC 15 - 3Sum on LeetCode](https://leetcode.com/problems/3sum/)

---

## Problem Statement

Given an integer array `nums`, return all unique triplets:

```python
[nums[i], nums[j], nums[k]]
```

such that:

```python
i != j
i != k
j != k
```

and:

```python
nums[i] + nums[j] + nums[k] == 0
```

The result must not contain duplicate triplets.

---

## Examples

### Example 1

```python
nums = [-1, 0, 1, 2, -1, -4]
```

Output:

```python
[
    [-1, -1, 2],
    [-1, 0, 1]
]
```

### Example 2

```python
nums = [0, 1, 1]
```

Output:

```python
[]
```

### Example 3

```python
nums = [0, 0, 0]
```

Output:

```python
[[0, 0, 0]]
```

---

# Pattern Recognition

## Signals

Look for:

```text
Three numbers
Triplets
Target sum
Unique combinations
Avoid duplicate answers
```

## Pattern

```text
Sorting + Fix One Value + Opposite-Direction Two Pointers
```

## Recognition Shortcut

Ask:

```text
Can I fix one number and reduce the remaining problem to Two Sum II?
```

For a fixed value `nums[i]`, we need:

```python
nums[left] + nums[right] == -nums[i]
```

This is a sorted two-sum problem inside a loop.

---

# Core Mental Model

```text
3Sum

Fix one value
    +
Solve Two Sum on the remaining sorted suffix
```

Example:

```python
nums[i] = -1
```

Then the remaining two values must satisfy:

```python
nums[left] + nums[right] = 1
```

because:

```python
-1 + nums[left] + nums[right] = 0
```

---

# Why Sorting Is Essential

Sorting provides three benefits.

## 1. Predictable Pointer Movement

```text
Total too small -> move left to a larger value
Total too large -> move right to a smaller value
```

## 2. Easy Duplicate Detection

Equal values become adjacent, so duplicate fixed values and duplicate pointer values can be skipped.

## 3. Early Exit

Once the fixed value becomes positive, three values from that position onward cannot sum to zero.

---

# Brute Force Approach

## Idea

Check every possible triplet.

```python
class Solution:
    def threeSum(self, nums):
        result = set()
        n = len(nums)

        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    if nums[i] + nums[j] + nums[k] == 0:
                        result.add(tuple(sorted([nums[i], nums[j], nums[k]])))

        return [list(triplet) for triplet in result]
```

## Complexity

```text
Time:  O(n^3)
Space: O(number of answers)
```

This is too slow for large inputs.

---

# Optimal Approach

## Algorithm

1. Sort `nums`.
2. Iterate through the array and fix `nums[i]`.
3. Skip `nums[i]` if it is the same as the previous fixed value.
4. Place `left` at `i + 1` and `right` at the end.
5. Calculate the three-number total.
6. If the total is too small, move `left` rightward.
7. If the total is too large, move `right` leftward.
8. If the total is zero:
   - Add the triplet.
   - Move both pointers.
   - Skip duplicate values at both pointers.

---

# Revision-Friendly Solution with Comments

```python
from typing import List


class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        # Sorting is essential because it:
        # 1. Enables predictable two-pointer movement.
        # 2. Places duplicates next to each other for easy skipping.
        nums.sort()

        result = []
        n = len(nums)

        # Fix the first value of each triplet.
        # We need at least two values after i, so n - 2 is sufficient.
        for i in range(n - 2):

            # Since the array is sorted, once nums[i] is positive,
            # every value to its right is also positive.
            # Three positive values cannot sum to zero.
            if nums[i] > 0:
                break

            # Skip duplicate fixed values.
            # Without this, the same triplets could be generated again.
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            # Solve a Two Sum II problem on the suffix after i.
            left = i + 1
            right = n - 1

            while left < right:
                total = nums[i] + nums[left] + nums[right]

                if total < 0:
                    # The total is too small.
                    # Move left to obtain a larger value.
                    left += 1

                elif total > 0:
                    # The total is too large.
                    # Move right to obtain a smaller value.
                    right -= 1

                else:
                    # A valid unique triplet has been found.
                    result.append([nums[i], nums[left], nums[right]])

                    # Move both pointers because the current pair
                    # has already been used with nums[i].
                    left += 1
                    right -= 1

                    # Skip duplicate values on the left.
                    # Compare with the value that was just used.
                    while left < right and nums[left] == nums[left - 1]:
                        left += 1

                    # Skip duplicate values on the right.
                    # Compare with the value that was just used.
                    while left < right and nums[right] == nums[right + 1]:
                        right -= 1

        return result
```

---

# Compact Solution

```python
class Solution:
    def threeSum(self, nums):
        nums.sort()
        result = []

        for i in range(len(nums) - 2):
            if nums[i] > 0:
                break

            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left = i + 1
            right = len(nums) - 1

            while left < right:
                total = nums[i] + nums[left] + nums[right]

                if total < 0:
                    left += 1
                elif total > 0:
                    right -= 1
                else:
                    result.append([nums[i], nums[left], nums[right]])
                    left += 1
                    right -= 1

                    while left < right and nums[left] == nums[left - 1]:
                        left += 1

                    while left < right and nums[right] == nums[right + 1]:
                        right -= 1

        return result
```

---

# Detailed Dry Run

Input:

```python
nums = [-1, 0, 1, 2, -1, -4]
```

After sorting:

```python
[-4, -1, -1, 0, 1, 2]
```

## Fix `i = 0`

```python
nums[i] = -4
left = 1
right = 5
```

### Pair `-1` and `2`

```python
-4 + -1 + 2 = -3
```

Too small, so:

```python
left += 1
```

The remaining totals are still too small. No triplet is found for fixed value `-4`.

## Fix `i = 1`

```python
nums[i] = -1
left = 2
right = 5
```

### Pair `-1` and `2`

```python
-1 + -1 + 2 = 0
```

Add:

```python
[-1, -1, 2]
```

Move both:

```python
left = 3
right = 4
```

### Pair `0` and `1`

```python
-1 + 0 + 1 = 0
```

Add:

```python
[-1, 0, 1]
```

Move both. The pointers meet.

## Fix `i = 2`

```python
nums[2] == nums[1] == -1
```

Skip this fixed value to avoid regenerating the same triplets.

Final result:

```python
[
    [-1, -1, 2],
    [-1, 0, 1]
]
```

---

# Duplicate Handling

Duplicate handling is the most important implementation detail in 3Sum.

## Duplicate Rule 1: Skip Repeated Fixed Values

```python
if i > 0 and nums[i] == nums[i - 1]:
    continue
```

Why?

If the previous iteration already fixed `-1`, fixing another adjacent `-1` would explore the same suffix pattern and could generate duplicate triplets.

## Duplicate Rule 2: Skip Repeated Left Values After a Match

```python
while left < right and nums[left] == nums[left - 1]:
    left += 1
```

The comparison uses `left - 1` because `left` was already incremented after adding the triplet.

## Duplicate Rule 3: Skip Repeated Right Values After a Match

```python
while left < right and nums[right] == nums[right + 1]:
    right -= 1
```

The comparison uses `right + 1` because `right` was already decremented after adding the triplet.

---

# Why Move Both Pointers After Finding a Triplet?

Once:

```python
nums[i] + nums[left] + nums[right] == 0
```

that exact pair has already been used with the fixed value.

Keeping either pointer unchanged cannot produce a new unique pair with the same opposite value.

Therefore:

```python
left += 1
right -= 1
```

Then duplicate values are skipped.

---

# Why We Can Stop When `nums[i] > 0`

The array is sorted.

If:

```python
nums[i] > 0
```

then:

```python
nums[left] >= nums[i] > 0
nums[right] >= nums[left] > 0
```

All three values are positive, so their sum cannot equal zero.

Therefore:

```python
if nums[i] > 0:
    break
```

is a safe optimization.

Do not stop when `nums[i] == 0`, because:

```python
[0, 0, 0]
```

is a valid triplet.

---

# Loop Invariants

## Outer Loop

For each `i`, all unique triplets whose first sorted value occurs before `i` have already been processed.

## Inner Loop

For the fixed `nums[i]`, any remaining valid pair lies within:

```text
[left, right]
```

The sorted order makes each pointer movement safe:

```text
Total too small -> discard current left value
Total too large -> discard current right value
```

---

# Complexity Analysis

Let `n` be the length of `nums`.

## Time Complexity

Sorting:

```text
O(n log n)
```

For each fixed index, the two pointers scan the remaining suffix in linear time:

```text
O(n^2)
```

Overall:

```text
O(n^2)
```

The quadratic two-pointer phase dominates the sorting cost.

## Space Complexity

Excluding the output and sorting implementation details:

```text
O(1)
```

Python sorting may use additional internal memory. The output itself can contain many triplets and is not counted as auxiliary space in the usual interview analysis.

---

# LC 167 vs LC 15

## LC 167 - Two Sum II

```text
Array is sorted
Target is given
Use left and right pointers once
Time: O(n)
```

## LC 15 - 3Sum

```text
Sort the array
Fix one value
Use LC 167-style two pointers on the remaining suffix
Repeat for each unique fixed value
Time: O(n^2)
```

## Memory Rule

```text
3Sum = Fix One + Two Sum II + Duplicate Handling
```

---

# Common Mistakes

## Mistake 1: Not Sorting

Without sorting:

- Pointer movement is not predictable.
- Duplicate values are difficult to skip correctly.

## Mistake 2: Forgetting to Skip Duplicate `i` Values

This can generate duplicate triplets.

```python
if i > 0 and nums[i] == nums[i - 1]:
    continue
```

## Mistake 3: Skipping Duplicates Before Recording the Match

First add the valid triplet, then move both pointers, then skip duplicates.

## Mistake 4: Moving Only One Pointer After a Match

Move both pointers after recording a valid triplet.

## Mistake 5: Returning Indices

Unlike LC 1 and LC 167, LC 15 asks for the triplet values.

## Mistake 6: Using a Set of Triplets Instead of Learning Duplicate Handling

A set can remove duplicate results, but it hides the important sorted duplicate-skipping pattern and may use more memory.

## Mistake 7: Breaking When `nums[i] >= 0`

Incorrect:

```python
if nums[i] >= 0:
    break
```

This would miss:

```python
[0, 0, 0]
```

Correct:

```python
if nums[i] > 0:
    break
```

## Mistake 8: Using the Same Element Twice

Initialize:

```python
left = i + 1
```

so that all three positions are distinct.

## Mistake 9: Incorrect Duplicate Comparisons

After moving pointers:

```python
left += 1
right -= 1
```

compare:

```python
nums[left] == nums[left - 1]
nums[right] == nums[right + 1]
```

---

# Interview Explanation

> I first sort the array so that pointer movement is predictable and duplicates become adjacent. I then fix one value at index `i` and use two pointers on the remaining suffix. If the three-number total is too small, I move the left pointer to a larger value. If it is too large, I move the right pointer to a smaller value. When the total is zero, I record the triplet, move both pointers, and skip duplicate values. I also skip duplicate fixed values. Sorting takes O(n log n), and the fixed-value plus two-pointer scan takes O(n^2), so the total time is O(n^2).

---

# Reusable Fix-One Template

```python
items.sort()
result = []

for i in range(len(items) - 2):
    if i > 0 and items[i] == items[i - 1]:
        continue

    left = i + 1
    right = len(items) - 1

    while left < right:
        total = items[i] + items[left] + items[right]

        if total < target:
            left += 1
        elif total > target:
            right -= 1
        else:
            result.append([items[i], items[left], items[right]])
            left += 1
            right -= 1

            while left < right and items[left] == items[left - 1]:
                left += 1

            while left < right and items[right] == items[right + 1]:
                right -= 1
```

For LC 15:

```python
target = 0
```

---

# What to Memorize

```text
Signal:
Three numbers + target sum + unique triplets

Pattern:
Sort + Fix One + Opposite-Direction Two Pointers

Reduction:
nums[left] + nums[right] = -nums[i]

Movement:
Total too small -> left += 1
Total too large -> right -= 1
Total equals zero -> record, move both, skip duplicates

Duplicate handling:
Skip repeated fixed values
Skip repeated left and right values after a match

Time:
O(n^2)
```

---

# Pattern Takeaway

LC 15 is the culmination of the core Two-Pointer module:

```text
A larger combination problem can sometimes be reduced by fixing one value
and solving a smaller sorted pair problem.
```

The core equation is:

```text
3Sum = Fix One + Two Sum II + Duplicate Handling
```

---

# Progress

```text
Two Pointers

[x] LC 125 - Valid Palindrome
[x] LC 167 - Two Sum II
[x] LC 283 - Move Zeroes
[x] LC 11  - Container With Most Water
[x] LC 15  - 3Sum

Core Two-Pointer problem set: Complete
Next roadmap topic: Sliding Window
```
