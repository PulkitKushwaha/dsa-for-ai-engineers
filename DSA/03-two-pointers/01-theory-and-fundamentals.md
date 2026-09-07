# Two Pointers: Theory and Fundamentals

> Two Pointers is not a single algorithm.
>
> It is a problem-solving technique in which two positions are maintained and moved according to information gained during traversal.
>
> The central question is:
>
> **What does the current comparison tell me about which pointer should move?**

---

# 1. Why Two Pointers Exists

A common brute-force approach checks every possible pair:

```python
for i in range(len(nums)):
    for j in range(i + 1, len(nums)):
        # examine nums[i] and nums[j]
```

This usually takes:

```text
O(n^2)
```

Two Pointers can sometimes avoid examining every pair by using structure in the input or problem.

Examples of useful structure include:

- The array is sorted.
- Characters must match from opposite ends.
- Elements must be moved or compacted in place.
- One pointer can read while another writes.
- A slower and faster traversal can reveal a cycle or midpoint.

When each comparison lets us safely eliminate possibilities, two pointers can often reduce the time to:

```text
O(n)
```

---

# 2. Core Mental Model

Imagine two observers positioned in a sequence.

```text
left                              right
  |                                  |
 [1, 2, 3, 4, 5, 6, 7, 8]
```

They inspect values and move according to a rule.

Possible movements include:

```text
Move left forward
Move right backward
Move both inward
Move a read pointer
Move a write pointer
Move one pointer faster than another
```

Two Pointers is useful only when the movement rule is logically safe.

Do not move a pointer because a template says so. Move it because the current state proves that some possibilities cannot produce the answer.

---

# 3. The Three Main Two-Pointer Families

## 3.1 Opposite-Direction Pointers

Pointers begin at opposite ends and move toward each other.

```python
left = 0
right = len(items) - 1

while left < right:
    # inspect items[left] and items[right]
```

Visual model:

```text
left                         right
  |                             |
 [a, b, c, d, e, f, g, h]
       ->             <-
```

### Common Signals

- Sorted array
- Pair sum
- Palindrome
- Compare first and last values
- Maximize or minimize using both ends
- Shrink a search range

### Representative Problems

- LC 125 - Valid Palindrome
- LC 167 - Two Sum II
- LC 11 - Container With Most Water
- LC 15 - 3Sum, after fixing one number

### Main Question

```text
Based on the current comparison, can I safely discard the left value, the right value, or both?
```

---

## 3.2 Same-Direction Read and Write Pointers

Both pointers move from left to right, but they have different responsibilities.

```text
read pointer  -> examines every value
write pointer -> marks where the next valid value should be placed
```

Example structure:

```python
write = 0

for read in range(len(nums)):
    if should_keep(nums[read]):
        nums[write] = nums[read]
        write += 1
```

Visual model:

```text
write
  |
 [0, 1, 0, 3, 12]
        |
       read
```

### Common Signals

- Move elements
- Remove elements
- Remove duplicates
- Modify the array in place
- Preserve the order of selected values
- Compact valid values toward the front

### Representative Problems

- LC 283 - Move Zeroes
- LC 26 - Remove Duplicates from Sorted Array
- LC 27 - Remove Element

### Main Question

```text
Which pointer reads the input, and which pointer marks the next output position?
```

---

## 3.3 Fast and Slow Pointers

Both pointers follow the same path but move at different speeds.

```python
slow = start
fast = start

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
```

Visual model:

```text
slow -> one step
fast -> two steps
```

### Common Signals

- Detect a cycle
- Find a midpoint
- Linked list traversal
- Repeated deterministic path
- Find where a cycle begins

### Representative Problems

- LC 141 - Linked List Cycle
- LC 876 - Middle of the Linked List
- LC 202 - Happy Number, alternative approach

### Main Question

```text
Can different traversal speeds reveal a cycle, midpoint, or meeting point?
```

> Fast and Slow Pointers will be studied more deeply with Linked Lists. It belongs to the wider Two Pointers family, but we will not force linked-list problems into the current array-focused module.

---

# 4. Why Sorted Data Is Important

Sorted order gives direction.

Suppose:

```python
numbers = [2, 7, 11, 15]
target = 18
```

Start with:

```python
left = 0
right = len(numbers) - 1
```

Current sum:

```python
numbers[left] + numbers[right]
```

If the sum is too small, moving `right` left would make the sum smaller or equal. That cannot help us reach a larger target.

Therefore, we move:

```python
left += 1
```

If the sum is too large, moving `left` right would make the sum larger or equal. That cannot help us reach a smaller target.

Therefore, we move:

```python
right -= 1
```

This is the real reason Two Pointers works here:

```text
Sorted order makes pointer movement logically meaningful.
```

Without a useful ordering or invariant, moving a pointer may discard valid answers.

---

# 5. Hashing vs Two Pointers

The same broad problem can sometimes be solved using either hashing or two pointers.

## Example: Find a Pair with a Target Sum

### HashMap Approach

```text
Time: O(n)
Space: O(n)
Input need not be sorted
```

Mental question:

```text
Have I already seen the required complement?
```

### Two-Pointer Approach

```text
Time: O(n), if already sorted
Space: O(1)
Requires sorted order or the ability to sort
```

Mental question:

```text
Is the current result too small, too large, or correct?
```

## Decision Rule

Use hashing when:

- The data is unsorted and indices from the original array matter.
- Fast complement lookup is useful.
- Extra `O(n)` space is acceptable.

Consider Two Pointers when:

- The data is already sorted.
- Sorting is allowed and original order is not required.
- Constant auxiliary space is valuable.
- Pointer movement can eliminate possibilities safely.

> Two Pointers does not automatically beat hashing. The correct choice depends on input structure, output requirements, and allowed space.

---

# 6. Two Pointers on Strings

Strings are sequences and can be accessed by index.

For a palindrome:

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

The comparison is symmetric:

```text
first character  <-> last character
second character <-> second-last character
```

If both characters match, both positions are resolved and both pointers move inward.

This becomes more interesting when punctuation or spaces must be skipped. A pointer may move independently until it reaches a valid character.

---

# 7. Pointer Movement Is the Algorithm

Initializing pointers is easy:

```python
left = 0
right = len(items) - 1
```

The difficult part is deciding when to move each one.

For every problem, define these rules before coding:

```text
When does the left pointer move?
When does the right pointer move?
When do both move?
When does the algorithm stop?
What invariant remains true after every movement?
```

## Example: Palindrome

```text
Characters differ -> return False
Characters match  -> move both inward
```

## Example: Sorted Pair Sum

```text
Sum too small -> move left rightward
Sum too large -> move right leftward
Sum equals target -> answer found
```

## Example: Read and Write

```text
Read points to an invalid item -> move read only
Read points to a valid item   -> write it, then move both roles forward
```

---

# 8. The Invariant Mental Model

An invariant is a condition that remains true throughout the loop.

It explains why discarded positions no longer need to be considered.

## Palindrome Invariant

Before every iteration:

```text
All character pairs outside [left, right] have already been validated.
```

## Sorted Two Sum Invariant

Before every iteration:

```text
Any possible solution still lies inside the current [left, right] range.
```

## Read and Write Invariant

Before every iteration:

```text
All positions before write already contain the processed valid values.
```

Thinking in invariants prevents random pointer movement and off-by-one errors.

---

# 9. Common Loop Conditions

## `left < right`

Use when two different positions must be compared or selected.

```python
while left < right:
```

Common in:

- Pair selection
- Palindrome comparison
- Container problems

## `left <= right`

Use when the same middle position may still need processing.

```python
while left <= right:
```

This appears in some search and partitioning problems, but should not be used automatically.

## Full Read Traversal

```python
for read in range(len(nums)):
```

Use when every input value must be examined and a separate write pointer tracks output placement.

## Fast-Pointer Safety

For linked lists:

```python
while fast and fast.next:
```

This prevents accessing beyond the end before moving `fast` by two steps.

---

# 10. In-Place Modification

Two Pointers is often used when the problem requires:

```text
Modify the input array without creating another full array.
```

Example:

```python
nums[write] = nums[read]
```

This commonly provides:

```text
O(n) time
O(1) auxiliary space
```

However, in-place solutions mutate the input. During an interview, explicitly mention this tradeoff.

Ask:

```text
Am I allowed to modify the input?
```

---

# 11. When Two Pointers Does Not Apply

Do not use Two Pointers merely because the input is an array.

It may not apply when:

- Pointer movement cannot eliminate any possibilities safely.
- The array is unsorted and sorting would destroy required original indices.
- The problem needs arbitrary lookup rather than directional movement.
- The search space is not monotonic.
- Moving one pointer could skip an unknown valid answer.

In such cases, consider:

- Hashing
- Sliding Window
- Binary Search
- Stack
- Dynamic Programming
- Graph traversal

---

# 12. Two Pointers vs Sliding Window

These patterns can look similar because both may use `left` and `right`.

## Two Pointers

Pointers often represent two positions whose values are compared or manipulated.

Examples:

```text
Compare opposite ends
Find a pair
Read and write positions
```

## Sliding Window

Pointers usually represent the boundaries of a contiguous range.

Examples:

```text
Longest valid substring
Minimum valid subarray
Fixed-size contiguous segment
```

## Quick Distinction

```text
Comparing or coordinating positions -> Two Pointers
Maintaining a contiguous range       -> Sliding Window
```

Some problems combine both ideas. We classify them based on the main invariant being maintained.

---

# 13. Complexity Intuition

A Two-Pointer loop is commonly `O(n)` because each pointer moves in only one direction.

Example:

```python
while left < right:
    if condition:
        left += 1
    else:
        right -= 1
```

Even though two pointers exist, each can move at most `n` positions.

Total movements are bounded by approximately:

```text
2n
```

Big-O removes constants:

```text
O(2n) = O(n)
```

The important condition is that the pointers do not repeatedly move backward and redo work.

---

# 14. General Problem-Solving Framework

When you suspect Two Pointers, follow this sequence.

## Step 1: Identify the Input Structure

Ask:

```text
Is the input sorted?
Is it a string?
Must the array be modified in place?
Is it a linked list?
```

## Step 2: Identify Pointer Roles

Choose among:

```text
left and right
read and write
slow and fast
```

## Step 3: Define the Invariant

Ask:

```text
What remains true after each movement?
```

## Step 4: Define Movement Rules

Write in plain language:

```text
If condition A, move left.
If condition B, move right.
If condition C, move both.
```

## Step 5: Define the Stop Condition

Examples:

```python
left < right
read < len(nums)
fast and fast.next
```

## Step 6: Check Edge Cases

Common cases:

- Empty input
- One element
- Two elements
- All values equal
- No valid answer
- Duplicate values
- Already valid input

## Step 7: Validate Complexity

Confirm that pointers move monotonically and total movement is linear.

---

# 15. Representative Pattern Map

## Opposite Ends

```text
Signals:
Sorted pair, palindrome, compare ends

Pointers:
left, right

Movement:
toward each other
```

## Read and Write

```text
Signals:
Remove, move, compact, in place

Pointers:
read, write

Movement:
both move forward with different responsibilities
```

## Fast and Slow

```text
Signals:
Cycle, midpoint, linked list

Pointers:
slow, fast

Movement:
same direction at different speeds
```

---

# 16. Common Mistakes

## Mistake 1: Moving the Wrong Pointer

Do not memorize movement directions without understanding why they eliminate impossible answers.

## Mistake 2: Moving Both Pointers Unconditionally

Some problems require only one pointer to move based on the current result.

## Mistake 3: Using Two Pointers on Unsorted Pair-Sum Data

The rule:

```text
sum too small -> move left
sum too large -> move right
```

requires sorted order.

## Mistake 4: Losing Original Indices by Sorting

If the problem requires original positions, sorting may require preserving index-value pairs or choosing hashing instead.

## Mistake 5: Incorrect Loop Boundary

Confusing:

```python
left < right
```

with:

```python
left <= right
```

can cause self-pairing, missed middle values, or out-of-range access.

## Mistake 6: Forgetting Input Mutation

Read-write pointer solutions often modify the input array.

## Mistake 7: Assuming Two Pointers Always Means Opposite Ends

Two Pointers also includes:

- Same-direction read/write
- Fast/slow traversal
- Fixed value plus two moving positions

---

# 17. Interview Explanation Template

Use this structure:

```text
The brute-force approach examines ______ and takes ______ time.

The input has the useful property ______.

I will maintain pointers representing ______ and ______.

When ______ happens, I move ______ because ______ can no longer lead to a valid answer.

Each pointer moves in only one direction, so the total time is O(n).
The auxiliary space is O(1), excluding the output.
```

## Example: Sorted Pair Sum

```text
The brute-force approach checks every pair and takes O(n^2) time. Because the array is sorted, the current sum tells me which side must move. If the sum is too small, I move the left pointer to increase it. If the sum is too large, I move the right pointer to decrease it. Each pointer moves inward at most n times, giving O(n) time and O(1) extra space.
```

---

# 18. What to Memorize

Do not memorize complete problem solutions. Memorize the recognition chain.

```text
Sorted pair or compare ends
-> Opposite-direction pointers

Remove, move, compact, in place
-> Read and write pointers

Cycle or midpoint
-> Fast and slow pointers
```

For every Two-Pointer problem, ask:

```text
What does the current comparison prove?
Which pointer can move safely?
What invariant remains true?
```

---

# 19. Module Roadmap

This theory file begins the Two Pointers module.

```text
03-two-pointers/
├── 01-theory-and-fundamentals.md
├── 02-pattern-recognition.md
├── 03-python-templates.md
├── 04-leetcode-roadmap.md
└── solutions/
```

Planned core problems:

```text
LC 125 - Valid Palindrome
LC 167 - Two Sum II
LC 283 - Move Zeroes
LC 11  - Container With Most Water
LC 15  - 3Sum
```

Potential supporting problems may be added only if they teach a distinct Two-Pointer variation without expanding the roadmap unnecessarily.

---

# Final Takeaway

Two Pointers is a technique for converting comparison information into directional movement.

The real pattern is not:

```text
Use two variables.
```

The real pattern is:

```text
Use the current state to eliminate possibilities without checking them individually.
```

Hashing asks:

```text
What have I already seen?
```

Two Pointers asks:

```text
Which pointer should move, and why is that movement safe?
```
