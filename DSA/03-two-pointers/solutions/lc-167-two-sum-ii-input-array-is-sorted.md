# LC 167 - Two Sum II: Input Array Is Sorted

## Problem Link

[Open LC 167 - Two Sum II on LeetCode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

---

## Problem Statement

You are given a **1-indexed** array of integers `numbers` that is already sorted in non-decreasing order.

Find two distinct numbers whose sum equals `target` and return their indices as:

```python
[index1, index2]
```

The returned indices must satisfy:

```text
1 <= index1 < index2 <= len(numbers)
```

You may assume that exactly one valid solution exists, and you may not use the same element twice.

The solution should use constant additional space.

---

## Examples

### Example 1

```python
numbers = [2, 7, 11, 15]
target = 9
```

Output:

```python
[1, 2]
```

Explanation:

```text
2 + 7 = 9
```

The values are at zero-based indices `0` and `1`, so the required one-based answer is:

```python
[1, 2]
```

### Example 2

```python
numbers = [2, 3, 4]
target = 6
```

Output:

```python
[1, 3]
```

Explanation:

```text
2 + 4 = 6
```

### Example 3

```python
numbers = [-1, 0]
target = -1
```

Output:

```python
[1, 2]
```

---

# Pattern Recognition

## Signals

Look for:

```text
Sorted array
Find a pair
Target sum
Two distinct positions
Constant extra space
```

## Pattern

```text
Two Pointers -> Opposite-Direction Pointers
```

## Recognition Shortcut

When the array is sorted and the problem asks for a pair with a target sum, ask:

```text
Is the current sum too small, too large, or equal to the target?
```

The answer determines which pointer moves.

## Core Insight

```text
Current sum too small -> move left rightward to increase the sum
Current sum too large -> move right leftward to decrease the sum
Current sum equals target -> return the indices
```

---

# Why Sorted Order Matters

Suppose:

```python
numbers = [2, 7, 11, 15]
```

Because the array is sorted:

```text
2 < 7 < 11 < 15
```

Moving the left pointer rightward selects a value that is greater than or equal to the previous left value. Therefore, the sum increases or stays the same.

Moving the right pointer leftward selects a value that is less than or equal to the previous right value. Therefore, the sum decreases or stays the same.

Without sorted order, pointer movements would not have predictable effects on the sum.

---

# Brute Force Approach

## Idea

Check every possible pair.

```python
class Solution:
    def twoSum(self, numbers, target):
        for left in range(len(numbers)):
            for right in range(left + 1, len(numbers)):
                if numbers[left] + numbers[right] == target:
                    return [left + 1, right + 1]
```

## Complexity Analysis

### Time Complexity

```text
O(n^2)
```

### Space Complexity

```text
O(1)
```

## Why It Is Not Optimal

The sorted order gives enough information to eliminate many pairs without checking them individually.

---

# Alternative Approach: HashMap

The complement-lookup technique from LC 1 also works:

```python
class Solution:
    def twoSum(self, numbers, target):
        lookup = {}

        for index, number in enumerate(numbers):
            needed = target - number

            if needed in lookup:
                return [lookup[needed] + 1, index + 1]

            lookup[number] = index
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

This does not use the sorted property and does not satisfy the constant-extra-space goal as well as the Two-Pointer approach.

---

# Optimal Approach: Opposite-Direction Two Pointers

## Algorithm

1. Place `left` at the beginning of the array.
2. Place `right` at the end of the array.
3. Calculate the sum of the values at both pointers.
4. If the sum equals `target`, return the one-based indices.
5. If the sum is smaller than `target`, move `left` rightward.
6. If the sum is larger than `target`, move `right` leftward.
7. Continue until the pair is found.

---

# Optimal Solution

```python
class Solution:
    def twoSum(self, numbers, target):
        left = 0
        right = len(numbers) - 1

        while left < right:
            current_sum = numbers[left] + numbers[right]

            if current_sum == target:
                return [left + 1, right + 1]

            if current_sum < target:
                left += 1
            else:
                right -= 1
```

---

# Why Each Pointer Movement Is Safe

## Case 1: Current Sum Is Too Small

```python
current_sum < target
```

We need a larger sum.

Because the array is sorted, moving `right` leftward would select a smaller or equal value and make the sum even smaller or unchanged.

Therefore, discard the current left value:

```python
left += 1
```

## Case 2: Current Sum Is Too Large

```python
current_sum > target
```

We need a smaller sum.

Because the array is sorted, moving `left` rightward would select a larger or equal value and make the sum even larger or unchanged.

Therefore, discard the current right value:

```python
right -= 1
```

## Case 3: Current Sum Matches

```python
current_sum == target
```

Return:

```python
[left + 1, right + 1]
```

The `+1` conversion is needed because the problem expects one-based indices.

---

# Detailed Dry Run

Input:

```python
numbers = [2, 7, 11, 15]
target = 9
```

Initially:

```python
left = 0
right = 3
```

## Iteration 1

```text
L           R
2   7   11  15
```

Current sum:

```python
2 + 15 = 17
```

Since:

```python
17 > 9
```

we need a smaller sum:

```python
right -= 1
```

Now:

```python
right = 2
```

## Iteration 2

```text
L       R
2   7   11  15
```

Current sum:

```python
2 + 11 = 13
```

Since:

```python
13 > 9
```

move `right` again:

```python
right = 1
```

## Iteration 3

```text
L   R
2   7   11  15
```

Current sum:

```python
2 + 7 = 9
```

The target is found.

Zero-based indices:

```python
[0, 1]
```

Required one-based indices:

```python
[1, 2]
```

---

# Dry Run When the Sum Is Too Small

Input:

```python
numbers = [2, 7, 11, 15]
target = 26
```

Initially:

```python
2 + 15 = 17
```

Since `17 < 26`, we need a larger sum.

Move:

```python
left += 1
```

Next sum:

```python
7 + 15 = 22
```

Still too small, so move `left` again:

```python
11 + 15 = 26
```

Target found.

Return:

```python
[3, 4]
```

---

# Loop Invariant

Before every iteration:

```text
If a valid pair exists, it remains within the current [left, right] range.
```

When a pointer moves, the eliminated value cannot participate in a valid pair with any remaining value under the current condition.

This invariant is what makes the algorithm correct.

---

# Why `left < right`?

The problem requires two distinct elements.

```python
while left < right:
```

ensures that the same index is never used twice.

Using:

```python
left <= right
```

could allow both pointers to refer to the same element.

---

# Complexity Analysis

Let `n` be the number of elements.

## Time Complexity

```text
O(n)
```

`left` only moves rightward and `right` only moves leftward. Each pointer moves at most `n` positions.

## Space Complexity

```text
O(1)
```

Only two pointers and the current sum are stored.

---

# LC 1 vs LC 167

## LC 1 - Two Sum

```text
Input: Unsorted
Primary approach: HashMap
Question: Have I already seen the complement?
Time: O(n)
Space: O(n)
Returns original indices
```

## LC 167 - Two Sum II

```text
Input: Sorted
Primary approach: Opposite-direction Two Pointers
Question: Is the current sum too small or too large?
Time: O(n)
Space: O(1)
Returns one-based indices
```

## Memory Rule

```text
Unsorted pair sum + original indices -> HashMap
Sorted pair sum -> Two Pointers
```

---

# Common Mistakes

## Mistake 1: Forgetting One-Based Indices

Incorrect:

```python
return [left, right]
```

Correct:

```python
return [left + 1, right + 1]
```

## Mistake 2: Moving the Wrong Pointer

```text
Sum too small -> move left
Sum too large -> move right
```

Do not reverse these movements.

## Mistake 3: Moving Both Pointers Before Finding the Target

Only one pointer should move when the sum is too small or too large.

## Mistake 4: Using Two Pointers Without Sorted Input

The movement logic depends on sorted order. Without ordering, the effect of pointer movement is unpredictable.

## Mistake 5: Using `left <= right`

This can allow one element to be used twice. Use:

```python
left < right
```

## Mistake 6: Sorting an Unsorted Array When Original Indices Matter

Sorting changes positions. LC 167 is already sorted and asks for positions in that sorted array, so this issue does not arise here.

## Mistake 7: Confusing Values with Indices

Return positions, not the pair values.

---

# Interview Explanation

> The brute-force solution checks every pair and takes O(n^2) time. Because the array is sorted, I can place one pointer at each end. If the current sum is too small, I move the left pointer to select a larger value. If the sum is too large, I move the right pointer to select a smaller value. If it matches the target, I return the one-based indices. Each pointer moves in only one direction, so the solution takes O(n) time and O(1) extra space.

---

# Reusable Sorted Pair Template

```python
left = 0
right = len(numbers) - 1

while left < right:
    current = numbers[left] + numbers[right]

    if current == target:
        return answer

    if current < target:
        left += 1
    else:
        right -= 1
```

---

# What to Memorize

```text
Signal:
Sorted array + pair sum + target

Pattern:
Opposite-direction Two Pointers

Question:
Is the current sum too small, too large, or equal?

Movement:
Too small -> left += 1
Too large -> right -= 1
Equal -> return answer

Time:
O(n)

Space:
O(1)
```

---

# Pattern Takeaway

LC 167 demonstrates the central Two-Pointer principle:

```text
Use sorted order to turn a comparison into a safe pointer movement.
```

The algorithm does not search for complements in a HashMap. Instead, the current sum reveals which side can be eliminated.

---

# Progress

```text
Two Pointers

[x] LC 125 - Valid Palindrome
[x] LC 167 - Two Sum II
[ ] LC 283 - Move Zeroes
[ ] LC 11  - Container With Most Water
[ ] LC 15  - 3Sum
```
