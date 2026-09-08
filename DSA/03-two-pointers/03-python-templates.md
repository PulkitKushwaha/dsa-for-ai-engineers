# Two Pointers - Python Templates

The purpose of this file is not to memorize solutions.

The purpose is to memorize reusable skeletons.

When a pattern is recognized:

```text
Recognize Pattern
↓
Pick Template
↓
Customize Logic
```

---

# Template 1: Opposite Direction Pointers

## When To Use

Signals:

```text
Sorted Array
Pair Sum
Palindrome
Compare Ends
Container Problems
```

---

## Template

```python
left = 0
right = len(items) - 1

while left < right:

    if condition_1:
        left += 1

    elif condition_2:
        right -= 1

    else:
        left += 1
        right -= 1
```

---

## Mental Model

```text
Left starts from beginning.
Right starts from end.

Comparison tells us
which side should move.
```

---

## Example

```text
LC 125 - Valid Palindrome
LC 167 - Two Sum II
```

---

# Template 2: Palindrome Template

## When To Use

Signals:

```text
Palindrome
Compare Ends
Mirror Comparison
```

---

## Template

```python
left = 0
right = len(s) - 1

while left < right:

    if s[left] != s[right]:
        return False

    left += 1
    right -= 1

return True
```

---

## Mental Model

```text
Compare mirror positions.

Match?
Move inward.

Mismatch?
Stop immediately.
```

---

# Template 3: Sorted Pair Sum

## When To Use

Signals:

```text
Sorted Array
Target Sum
Find Pair
```

---

## Template

```python
left = 0
right = len(nums) - 1

while left < right:ums[left] + nums[right]

    if current == target:
        return [left, right]

    elif current < target:
        left += 1

    else:
        right -= 1
```

---

## Mental Model

```text
Sum Too Small
↓
Need Larger Number
↓
Move Left

Sum Too Large
↓
Need Smaller Number
↓
Move Right
```

---

## Example

```text
LC 167 - Two Sum II
```

---

# Template 4: Read / Write Pointer

## When To Use

Signals:

```text
Move Elements
Remove Items
Compact Array
Modify In Place
```

---

## Template

```python
write = 0

for read in range(len(nums)):

    if is_valid(nums[read]):

        nums[write] = nums[read]
        write += 1
```

---

## Mental Model

```text
Read examines everything.

Write marks:
"Where should the next valid value go?"
```

---

## Example

```text
LC 283 - Move Zeroes
LC 26 - Remove Duplicates
```

---

# Template 5: Move Zeroes

## Template

```python
write = 0

for read in range(len(nums)):

    if nums[read] != 0:

        nums[write], nums[read] = (
            nums[read],
            nums[write]
        )

        write += 1
```

---

## Mental Model

```text
Whenever a non-zero appears:

Push it forward.

Write pointer tracks
next non-zero location.
```

---

## Example

```text
LC 283 - Move Zeroes
```

---

# Template 6: Fast / Slow Pointer

## When To Use

Signals:

```text
Cycle
Middle Node
Linked List
```

---

## Template

```python
slow = head
fast = head

while fast and fast.next:

    slow = slow.next
    fast = fast.next.next
```

---

## Mental Model

```text
Slow = 1 step

Fast = 2 steps

If they meet:
Cycle exists
```

---

## Example

```text
LC 141 - Linked List Cycle
```

---

# Template 7: Find Middle Node

## Template

```python
slow = head
fast = head

while fast and fast.next:

    slow = slow.next
    fast = fast.next.next

return slow
```

---

## Mental Model

```text
Fast reaches end.

Slow reaches middle.
```

---

## Example

```text
LC 876 - Middle of Linked List
```

---

# Template 8: Fixed Value + Two Pointers

Used in:

```text
3Sum
4Sum
```

---

## Template

```python
nums.sort()

for i in range(len(nums)):

    left = i + 1
    right = len(nums) - 1

    while left < right:

        current = (
            nums[i]
            + nums[left]
            + nums[right]
        )

        if current == target:
            # answer found

        elif current < target:
            left += 1

        else:
            right -= 1
```

---

## Mental Model

```text
Fix One Value

↓

Solve Remaining Problem
Using Two Pointers
```

---

## Example

```text
LC 15 - 3Sum
LC 18 - 4Sum
```

---

# Template Selection Cheat Sheet

## Compare Ends?

```text
Use:
Opposite Direction
```

---

## Palindrome?

```text
Use:
Palindrome Template
```

---

## Sorted Pair Sum?

```text
Use:
Sorted Pair Template
```

---

## Move Elements?

```text
Use:
Read / Write
```

---

## Cycle?

```text
Use:
Fast / Slow
```

---

## Triplets?

```text
Use:
Fix One + Two Pointers
```

---

# What To Memorize

Do NOT memorize problems.

Memorize:

```text
Pattern
↓
Template
```

---

```text
Palindrome
↓
Opposite Ends
```

---

```text
Target Sum In Sorted Array
↓
Opposite Ends
```

---

```text
Move Elements
↓
Read / Write
```

---

```text
Cycle
↓
Fast / Slow
```

---

```text
3Sum
↓
Fix One + Two Pointers
```

---

# Golden Rule

Hashing asks:

```text
Have I seen this before?
```

Two Pointers asks:

```text
Which pointer should move?
```

That question is the foundation of every Two Pointer problem.