# LC 11 - Container With Most Water

## Problem Link

[Open LC 11 - Container With Most Water on LeetCode](https://leetcode.com/problems/container-with-most-water/)

---

## Problem Statement

You are given an integer array `height` of length `n`.

Each value represents the height of a vertical line drawn at that index. Choose two lines that, together with the horizontal axis, form a container.

Return the maximum amount of water that any such container can hold.

The container cannot be tilted.

---

## Examples

### Example 1

```python
height = [1, 8, 6, 2, 5, 4, 8, 3, 7]
```

Output:

```python
49
```

The best container uses the lines at indices `1` and `8`:

```text
Height = min(8, 7) = 7
Width  = 8 - 1 = 7
Area   = 7 * 7 = 49
```

### Example 2

```python
height = [1, 1]
```

Output:

```python
1
```

Calculation:

```text
Height = min(1, 1) = 1
Width  = 1
Area   = 1
```

---

# Pattern Recognition

## Signals

Look for:

```text
Choose two boundaries
Maximize area
Width depends on distance between positions
Height is limited by the smaller boundary
Search from both ends
```

## Pattern

```text
Two Pointers -> Opposite-Direction Pointers -> Bottleneck Elimination
```

## Recognition Shortcut

Ask:

```text
Which boundary is limiting the current result?
```

The shorter line limits the container height. Therefore, only replacing the shorter line gives any possibility of increasing the area after the width decreases.

## Core Mental Model

```text
Area = width * limiting height

Width           = right - left
Limiting height = min(height[left], height[right])
```

The shorter wall is the bottleneck.

---

# Understanding the Area Formula

For two lines at indices `left` and `right`:

```python
width = right - left
```

The water height is limited by the shorter line:

```python
container_height = min(height[left], height[right])
```

Therefore:

```python
area = width * container_height
```

or:

```python
area = (right - left) * min(height[left], height[right])
```

## Why Use the Shorter Height?

Suppose the two walls have heights:

```text
Left wall  = 3
Right wall = 8
```

Water cannot rise to height `8` because it would spill over the wall of height `3`.

Therefore:

```python
min(3, 8) = 3
```

is the usable container height.

---

# Brute Force Approach

## Idea

Check every possible pair of lines, calculate its area, and retain the maximum.

## Solution

```python
class Solution:
    def maxArea(self, height):
        maximum = 0

        for left in range(len(height)):
            for right in range(left + 1, len(height)):
                width = right - left
                container_height = min(height[left], height[right])
                area = width * container_height
                maximum = max(maximum, area)

        return maximum
```

## Complexity Analysis

### Time Complexity

```text
O(n^2)
```

Every pair of lines is examined.

### Space Complexity

```text
O(1)
```

## Why It Is Not Optimal

The brute-force approach ignores the information provided by the current shorter wall. Two Pointers uses that bottleneck to eliminate pairs that cannot improve the current area.

---

# Key Insight: Move the Shorter Wall

Start with the widest possible container:

```python
left = 0
right = len(height) - 1
```

At every step, moving either pointer decreases the width.

Therefore, a smaller width can produce a larger area only if the limiting height improves enough.

The current limiting height is the shorter wall.

This leads to the movement rule:

```text
Left wall shorter  -> move left
Right wall shorter -> move right
Equal heights      -> move either one
```

---

# Why Moving the Taller Wall Cannot Help

Suppose:

```python
height[left] < height[right]
```

The current area is:

```python
height[left] * (right - left)
```

because the left wall is shorter.

If we move the right pointer inward while keeping the same left wall:

- The width becomes smaller.
- The container height can never exceed `height[left]`, because the unchanged left wall still limits it.

Therefore, the new area cannot be larger than the current area for that same left boundary.

The only potentially useful move is to replace the shorter left wall:

```python
left += 1
```

This may discover a taller boundary that compensates for the reduced width.

The same reasoning applies symmetrically when the right wall is shorter.

---

# Equal Heights

Suppose:

```python
height[left] == height[right]
```

Both walls limit the current area equally.

Moving either pointer decreases the width while the unchanged wall continues to cap the height at the same value.

Therefore, either pointer can be moved safely.

These are both valid:

```python
if height[left] <= height[right]:
    left += 1
else:
    right -= 1
```

and:

```python
if height[left] < height[right]:
    left += 1
else:
    right -= 1
```

The second version moves `right` when the heights are equal. The first version moves `left`. Both are correct.

## Interview Answer

> When both heights are equal, both are equally limiting. The width will decrease no matter which pointer moves, so there is no advantage to choosing one side over the other. We can move either pointer and preserve correctness.

---

# Optimal Algorithm

1. Place `left` at index `0`.
2. Place `right` at the final index.
3. Calculate the current width.
4. Calculate the area using the shorter wall.
5. Update the maximum area.
6. Move the pointer at the shorter wall inward.
7. If the walls are equal, move either pointer.
8. Continue until the pointers meet.

---

# Optimal Solution

```python
class Solution:
    def maxArea(self, height):
        left = 0
        right = len(height) - 1
        maximum = 0

        while left < right:
            width = right - left
            container_height = min(height[left], height[right])
            area = width * container_height

            maximum = max(maximum, area)

            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return maximum
```

---

# Concise Version

```python
class Solution:
    def maxArea(self, height):
        left = 0
        right = len(height) - 1
        maximum = 0

        while left < right:
            maximum = max(
                maximum,
                (right - left) * min(height[left], height[right])
            )

            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return maximum
```

For interviews and revision, the expanded version is often easier to explain.

---

# Detailed Dry Run

Input:

```python
height = [1, 8, 6, 2, 5, 4, 8, 3, 7]
```

Initially:

```python
left = 0
right = 8
maximum = 0
```

## Iteration 1

Boundary heights:

```python
height[left] = 1
height[right] = 7
```

Width:

```python
8 - 0 = 8
```

Area:

```python
min(1, 7) * 8 = 8
```

Update:

```python
maximum = 8
```

The left wall is shorter:

```python
1 < 7
```

Move:

```python
left += 1
```

## Iteration 2

Now:

```python
left = 1
right = 8
```

Boundary heights:

```python
height[left] = 8
height[right] = 7
```

Width:

```python
8 - 1 = 7
```

Area:

```python
min(8, 7) * 7 = 49
```

Update:

```python
maximum = 49
```

The right wall is shorter:

```python
7 < 8
```

Move:

```python
right -= 1
```

## Iteration 3

```python
left = 1
right = 7
```

Heights:

```python
8 and 3
```

Width:

```python
6
```

Area:

```python
min(8, 3) * 6 = 18
```

Maximum remains:

```python
49
```

Move the shorter right wall.

## Remaining Iterations

The pointers continue moving inward. No later area exceeds `49`.

Final result:

```python
49
```

---

# Pointer Movement Table

| Left Height | Right Height | Limiting Wall | Pointer Movement |
|---:|---:|---|---|
| Smaller | Larger | Left | `left += 1` |
| Larger | Smaller | Right | `right -= 1` |
| Equal | Equal | Both equally | Move either pointer |

---

# Loop Invariant

Before every iteration:

```text
maximum stores the largest area examined so far.

Any pair that could still improve the answer is represented within
the current pointer range or will be reached by moving the shorter wall.
```

When the shorter wall is discarded, no container using that same wall and a smaller width can exceed the area already considered with its widest remaining partner.

---

# Why Start at the Two Ends?

The initial pointers provide the maximum possible width:

```python
len(height) - 1
```

All later containers have smaller widths.

Starting at both ends allows the algorithm to progressively trade width for the possibility of a taller limiting wall.

---

# Complexity Analysis

Let `n` be the number of vertical lines.

## Time Complexity

```text
O(n)
```

Each iteration moves either `left` or `right` inward. Neither pointer reverses direction, so there are at most `n - 1` pointer movements.

## Space Complexity

```text
O(1)
```

Only pointer, width, area, and maximum variables are used.

---

# Why This Is Not a Sliding Window Problem

The pointers do form boundaries, but the algorithm is not maintaining a valid contiguous window with changing contents.

Instead, it evaluates a pair of boundaries and eliminates one boundary based on the bottleneck height.

```text
Boundary comparison and elimination -> Two Pointers
Contiguous-range state maintenance  -> Sliding Window
```

---

# Common Mistakes

## Mistake 1: Using the Taller Height

Incorrect:

```python
area = max(height[left], height[right]) * width
```

Water is limited by the shorter wall:

```python
area = min(height[left], height[right]) * width
```

## Mistake 2: Forgetting the Width

The width is the distance between indices:

```python
right - left
```

not the number of elements between them and not `right - left + 1`.

## Mistake 3: Moving the Taller Wall

Moving the taller wall reduces width while leaving the same shorter bottleneck in place. It cannot improve the area for that shorter boundary.

## Mistake 4: Moving Both Pointers Every Time

When the heights differ, move only the shorter wall. Moving both can skip a potentially optimal container.

## Mistake 5: Assuming Equal Heights Require Moving Both

When equal, moving either one is sufficient. Moving both can still work in some reasoning variants, but the standard and clearest implementation moves only one side.

## Mistake 6: Using `left <= right`

A container requires two distinct lines and positive width. Use:

```python
while left < right:
```

## Mistake 7: Confusing Height with Index

Use values for container height:

```python
height[left]
height[right]
```

Use indices for width:

```python
right - left
```

## Mistake 8: Checking Every Pair

The brute-force solution is `O(n^2)`. The shorter-wall proof is what enables the linear solution.

---

# Interview Explanation

> The area between two lines is the distance between their indices multiplied by the shorter height. I start with pointers at both ends to get the maximum possible width. After calculating the current area, I move the pointer at the shorter line because that line limits the water level. Moving the taller line would only reduce the width while keeping the same shorter bottleneck, so it cannot improve the area. Each pointer moves inward at most once per position, giving O(n) time and O(1) extra space.

---

# Reusable Bottleneck-Elimination Template

```python
left = 0
right = len(values) - 1
best = 0

while left < right:
    current = evaluate(values, left, right)
    best = max(best, current)

    if values[left] < values[right]:
        left += 1
    else:
        right -= 1

return best
```

Use this template only when you can prove that one boundary is the current bottleneck and discarding it is safe.

---

# Pattern Comparison

## LC 125 - Valid Palindrome

```text
Question:
Do the boundary characters match?

Movement:
Valid match -> move both
Invalid character -> move that pointer
```

## LC 167 - Two Sum II

```text
Question:
Is the current sum too small or too large?

Movement:
Too small -> move left
Too large -> move right
```

## LC 283 - Move Zeroes

```text
Question:
Where should the next valid value be written?

Movement:
Read always advances
Write advances for non-zero values
```

## LC 11 - Container With Most Water

```text
Question:
Which boundary limits the current area?

Movement:
Move the shorter wall
```

---

# What to Memorize

```text
Signal:
Two boundaries + maximize area + width * limiting height

Pattern:
Opposite-direction Two Pointers

Formula:
area = (right - left) * min(height[left], height[right])

Question:
Which wall is the bottleneck?

Movement:
Move the shorter wall
Equal heights -> move either wall

Time:
O(n)

Space:
O(1)
```

---

# Pattern Takeaway

LC 11 teaches **bottleneck elimination**.

The area is controlled by:

```text
Width and the shorter wall
```

Since every pointer movement reduces the width, the only useful possibility is to replace the wall currently limiting the height.

```text
Move the shorter wall because it is the only boundary whose replacement may increase the limiting height.
```

---

# Progress

```text
Two Pointers

[x] LC 125 - Valid Palindrome
[x] LC 167 - Two Sum II
[x] LC 283 - Move Zeroes
[x] LC 11  - Container With Most Water
[ ] LC 15  - 3Sum
```
