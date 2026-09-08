# LC 283 - Move Zeroes

## Problem Link

[Open LC 283 - Move Zeroes on LeetCode](https://leetcode.com/problems/move-zeroes/)

---

## Problem Statement

Given an integer array `nums`, move all zeroes to the end while maintaining the relative order of the non-zero elements.

The operation must be performed **in place**, without creating a copy of the complete array.

---

## Examples

### Example 1

```python
nums = [0, 1, 0, 3, 12]
```

After modification:

```python
[1, 3, 12, 0, 0]
```

### Example 2

```python
nums = [0]
```

After modification:

```python
[0]
```

### Example 3

```python
nums = [1, 2, 3]
```

After modification:

```python
[1, 2, 3]
```

---

# Pattern Recognition

## Signals

Look for:

```text
Move certain elements
Preserve relative order
Modify the array in place
Compact valid values
Push unwanted values to one side
```

## Pattern

```text
Two Pointers -> Same-Direction Read and Write Pointers
```

## Recognition Shortcut

Ask:

```text
Where should the next valid element be written?
```

## Pointer Roles

```text
Read pointer:
Examines every element.

Write pointer:
Tracks the position where the next non-zero element belongs.
```

---

# Mental Model

Imagine the array has two roles operating on it:

```text
Reader -> finds the next useful value
Writer -> places that value in the next correct position
```

For:

```python
[0, 1, 0, 3, 12]
```

The reader discovers non-zero values in this order:

```text
1, 3, 12
```

The writer packs them at the front:

```text
[1, 3, 12, _, _]
```

The remaining positions are then filled with zeroes.

---

# Requirements Hidden in the Problem

The solution must satisfy three conditions:

## 1. Move Zeroes to the End

```python
[0, 1, 0, 3, 12]
```

becomes:

```python
[1, 3, 12, 0, 0]
```

## 2. Preserve Relative Order

The non-zero values originally appear as:

```text
1, 3, 12
```

They must remain in that order.

## 3. Modify In Place

The original list must be changed rather than replaced by another complete result array.

---

# Brute Force Approach Using Extra Space

## Idea

1. Collect all non-zero values in another list.
2. Append enough zeroes.
3. Copy the result back into `nums`.

## Solution

```python
class Solution:
    def moveZeroes(self, nums):
        result = []

        for num in nums:
            if num != 0:
                result.append(num)

        while len(result) < len(nums):
            result.append(0)

        for index in range(len(nums)):
            nums[index] = result[index]
```

## Complexity Analysis

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(n)
```

## Why It Is Not Preferred

Although the time complexity is linear, the additional list violates the intended constant-extra-space requirement.

---

# Approach 1: Read, Write, Then Fill Zeroes

## Key Idea

Use a write pointer to compact all non-zero values at the front.

After the first pass:

```text
All positions before write contain the non-zero values in original order.
```

Then fill positions from `write` to the end with zeroes.

---

# Algorithm

1. Initialize `write = 0`.
2. Move `read` across the complete array.
3. Whenever `nums[read]` is non-zero:
   - Copy it to `nums[write]`.
   - Increment `write`.
4. After all non-zero values have been written, fill the remaining positions with zeroes.

---

# Solution: Overwrite and Fill

```python
class Solution:
    def moveZeroes(self, nums):
        write = 0

        for read in range(len(nums)):
            if nums[read] != 0:
                nums[write] = nums[read]
                write += 1

        while write < len(nums):
            nums[write] = 0
            write += 1
```

---

# Detailed Dry Run

Input:

```python
nums = [0, 1, 0, 3, 12]
```

Initially:

```python
write = 0
```

## Read Index 0

```python
nums[0] = 0
```

The value is zero, so it is skipped.

```python
write = 0
```

Array:

```python
[0, 1, 0, 3, 12]
```

## Read Index 1

```python
nums[1] = 1
```

The value is non-zero, so write it at index `0`:

```python
nums[write] = nums[read]
nums[0] = nums[1]
```

Array:

```python
[1, 1, 0, 3, 12]
```

Increment:

```python
write = 1
```

## Read Index 2

```python
nums[2] = 0
```

Skip it.

```python
write = 1
```

## Read Index 3

```python
nums[3] = 3
```

Write it at index `1`:

```python
nums[1] = 3
```

Array:

```python
[1, 3, 0, 3, 12]
```

Increment:

```python
write = 2
```

## Read Index 4

```python
nums[4] = 12
```

Write it at index `2`:

```python
nums[2] = 12
```

Array:

```python
[1, 3, 12, 3, 12]
```

Increment:

```python
write = 3
```

## Fill Remaining Positions

Everything before `write` is correct:

```text
[1, 3, 12, _, _]
```

Fill from index `3` onward:

```python
nums[3] = 0
nums[4] = 0
```

Final array:

```python
[1, 3, 12, 0, 0]
```

---

# Why Overwriting Does Not Lose Needed Data

When a non-zero value is copied from `read` to `write`, we always have:

```python
write <= read
```

The write pointer never moves ahead of the read pointer.

Therefore, the algorithm never overwrites an unprocessed value located to the right of `read`.

---

# Loop Invariant

During the first pass:

```text
All positions before write contain exactly the non-zero values
seen so far, in their original relative order.
```

The read pointer examines every value. The write pointer advances only when a non-zero value is found.

This invariant explains both correctness and order preservation.

---

# Approach 2: Swap Non-Zero Values Forward

Instead of overwriting values and filling zeroes in a second pass, swap every discovered non-zero value into the current write position.

## Solution

```python
class Solution:
    def moveZeroes(self, nums):
        write = 0

        for read in range(len(nums)):
            if nums[read] != 0:
                nums[write], nums[read] = nums[read], nums[write]
                write += 1
```

---

# How the Swap Version Works

Input:

```python
nums = [0, 1, 0, 3, 12]
```

## Read Index 0

Value is zero. Skip.

```python
write = 0
```

## Read Index 1

Value is `1`. Swap indices `0` and `1`:

```python
[1, 0, 0, 3, 12]
```

Increment:

```python
write = 1
```

## Read Index 2

Value is zero. Skip.

## Read Index 3

Value is `3`. Swap indices `1` and `3`:

```python
[1, 3, 0, 0, 12]
```

Increment:

```python
write = 2
```

## Read Index 4

Value is `12`. Swap indices `2` and `4`:

```python
[1, 3, 12, 0, 0]
```

Final result:

```python
[1, 3, 12, 0, 0]
```

---

# Why the Swap Version Preserves Order

The read pointer encounters non-zero values from left to right.

They are written to positions:

```text
0, 1, 2, ...
```

in the same discovery order.

Therefore, the relative order of non-zero elements remains unchanged.

---

# Comparing the Two Optimal Versions

## Overwrite and Fill

```python
nums[write] = nums[read]
```

Then fill remaining positions with zeroes.

Advantages:

- Clear separation between compaction and padding.
- Easy to reason about with the read/write invariant.

Tradeoff:

- Requires a second pass over the remaining suffix.

## Swap Version

```python
nums[write], nums[read] = nums[read], nums[write]
```

Advantages:

- Completes the transformation in one traversal.
- Naturally pushes zeroes backward.

Tradeoff:

- May perform unnecessary self-swaps when `write == read`.
- The mutation may initially be less intuitive.

Both approaches use:

```text
Time: O(n)
Space: O(1)
```

For learning the read/write concept, the overwrite-and-fill version is often easier to explain. The swap version is an elegant interview alternative.

---

# Optional Swap Optimization

Avoid swapping an element with itself:

```python
class Solution:
    def moveZeroes(self, nums):
        write = 0

        for read in range(len(nums)):
            if nums[read] != 0:
                if write != read:
                    nums[write], nums[read] = nums[read], nums[write]

                write += 1
```

This does not change the asymptotic complexity. Use it only if it improves the implementation for the situation.

---

# Complexity Analysis

Let `n` be the array length.

## Time Complexity

```text
O(n)
```

For the overwrite-and-fill approach, the first and second passes together process at most approximately `2n` positions:

```text
O(n) + O(n) = O(n)
```

The swap approach uses one traversal.

## Space Complexity

```text
O(1)
```

Only pointer variables are used. No additional array proportional to the input size is created.

---

# Why This Is a Two-Pointer Problem

The pointers do not begin at opposite ends.

They move in the same direction but perform different jobs:

```text
read  -> scans every element
write -> tracks the next destination for a non-zero element
```

This is the **read/write pointer pattern**.

---

# Common Mistakes

## Mistake 1: Incrementing `write` for Zeroes

Incorrect:

```python
for read in range(len(nums)):
    if nums[read] != 0:
        nums[write] = nums[read]

    write += 1
```

The write pointer should advance only when a valid non-zero element is placed.

## Mistake 2: Returning a New Array

The problem requires in-place modification. Modify `nums` directly.

LeetCode does not require returning the array from `moveZeroes`.

## Mistake 3: Forgetting to Fill the Remaining Suffix

After compaction, old values can remain at the end:

```python
[1, 3, 12, 3, 12]
```

The remaining positions must be set to zero in the overwrite version.

## Mistake 4: Sorting the Array

Sorting could move zeroes but would not necessarily preserve the relative order of non-zero elements. It also adds `O(n log n)` time.

## Mistake 5: Repeatedly Removing Zeroes

Operations such as list removal and insertion can shift many elements repeatedly and lead to quadratic behavior.

## Mistake 6: Confusing Pointer Roles

Remember:

```text
Read asks: What is this value?
Write asks: Where should the next valid value go?
```

## Mistake 7: Using Extra Space Without Mentioning the Tradeoff

An auxiliary-array solution is valid conceptually, but it misses the in-place requirement.

---

# Interview Explanation

> I use a read pointer to inspect every element and a write pointer to track where the next non-zero value should be placed. Whenever the read pointer finds a non-zero value, I write it at the write position and advance the write pointer. After all non-zero values have been compacted at the front in their original order, I fill the remaining suffix with zeroes. Each position is processed only a constant number of times, so the time complexity is O(n), and the extra space is O(1).

---

# Reusable Read/Write Template

```python
write = 0

for read in range(len(items)):
    if is_valid(items[read]):
        items[write] = items[read]
        write += 1

while write < len(items):
    items[write] = replacement_value
    write += 1
```

Use this pattern when:

```text
Some elements should be retained in order.
Retained elements should be compacted.
Remaining positions need a default value.
The modification must happen in place.
```

---

# Pattern Comparison

## LC 125 - Valid Palindrome

```text
Pointer type: Opposite directions
Question: Do mirrored characters match?
```

## LC 167 - Two Sum II

```text
Pointer type: Opposite directions
Question: Is the current sum too small or too large?
```

## LC 283 - Move Zeroes

```text
Pointer type: Same-direction read/write
Question: Where should the next valid value be written?
```

---

# What to Memorize

```text
Signal:
Move elements + preserve order + modify in place

Pattern:
Read and Write Pointers

Read pointer:
Examines every item

Write pointer:
Marks the next position for a valid item

Movement:
Zero -> move read only
Non-zero -> write or swap, then advance write

Time:
O(n)

Space:
O(1)
```

---

# Pattern Takeaway

LC 283 introduces the second major Two-Pointer family:

```text
Same-direction read/write pointers
```

The decisive mental question is:

```text
Where should the next valid element go?
```

This pattern will reappear in removing elements, removing duplicates from sorted arrays, partitioning, and in-place compaction problems.

---

# Progress

```text
Two Pointers

[x] LC 125 - Valid Palindrome
[x] LC 167 - Two Sum II
[x] LC 283 - Move Zeroes
[ ] LC 11  - Container With Most Water
[ ] LC 15  - 3Sum
```
