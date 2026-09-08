# LC 125 - Valid Palindrome

## Problem Link

[Open LC 125 - Valid Palindrome on LeetCode](https://leetcode.com/problems/valid-palindrome/)

---

## Problem Statement

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward.

Alphanumeric characters include:

- Letters from `a` to `z`
- Letters from `A` to `Z`
- Digits from `0` to `9`

Given a string `s`, return `True` if it is a palindrome. Otherwise, return `False`.

---

## Examples

### Example 1

```python
s = "A man, a plan, a canal: Panama"
```

Output:

```python
True
```

After ignoring spaces, punctuation, and capitalization:

```text
amanaplanacanalpanama
```

This reads the same forward and backward.

### Example 2

```python
s = "race a car"
```

Output:

```python
False
```

After cleaning:

```text
raceacar
```

This is not a palindrome.

### Example 3

```python
s = " "
```

Output:

```python
True
```

After removing the non-alphanumeric character, the string is empty. An empty string is considered a palindrome.

---

# Pattern Recognition

## Signals

Look for:

```text
Palindrome
Reads the same forward and backward
Compare mirrored positions
Ignore punctuation or spaces
Case-insensitive comparison
```

## Pattern

```text
Two Pointers -> Opposite-Direction Pointers
```

## Recognition Shortcut

A palindrome compares mirrored positions:

```text
First character       <-> Last character
Second character      <-> Second-last character
Third character       <-> Third-last character
```

This suggests:

```python
left = 0
right = len(s) - 1
```

## Core Question

```text
Are the next valid characters from the left and right equal?
```

---

# Mental Model

Imagine two readers:

```text
left reader  -> starts at the beginning
right reader -> starts at the end
```

They move toward each other.

Before comparing, each reader skips characters that do not matter, such as:

```text
Spaces
Commas
Colons
Other punctuation
```

When both pointers reach valid alphanumeric characters:

```text
Characters match    -> move both inward
Characters mismatch -> return False
```

---

# Approach 1: Create a Cleaned String

## Idea

Create a new string containing only lowercase alphanumeric characters. Compare it with its reverse.

## Solution

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        cleaned = []

        for char in s:
            if char.isalnum():
                cleaned.append(char.lower())

        cleaned = "".join(cleaned)
        return cleaned == cleaned[::-1]
```

## Complexity Analysis

### Time Complexity

```text
O(n)
```

The string is traversed, joined, and reversed.

### Space Complexity

```text
O(n)
```

A cleaned string and its reversed representation are created.

## Assessment

This approach is clear and valid, but it uses additional memory. The Two-Pointer solution compares characters directly and uses constant auxiliary space.

---

# Optimal Approach: Two Pointers

## Key Idea

Use one pointer at each end of the original string.

```python
left = 0
right = len(s) - 1
```

Before comparing characters:

1. Move `left` forward while it points to a non-alphanumeric character.
2. Move `right` backward while it points to a non-alphanumeric character.
3. Compare the lowercase valid characters.
4. If they differ, return `False`.
5. If they match, move both pointers inward.

---

# Optimal Solution

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1

        while left < right:
            while left < right and not s[left].isalnum():
                left += 1

            while left < right and not s[right].isalnum():
                right -= 1

            if s[left].lower() != s[right].lower():
                return False

            left += 1
            right -= 1

        return True
```

---

# Understanding `isalnum()`

The method:

```python
character.isalnum()
```

returns `True` when the character is a letter or digit.

Examples:

```python
'A'.isalnum()   # True
'z'.isalnum()   # True
'7'.isalnum()   # True
' '.isalnum()   # False
','.isalnum()   # False
':'.isalnum()   # False
```

Therefore:

```python
not s[left].isalnum()
```

means:

```text
The character at the left pointer is not a letter or digit.
```

---

# Understanding the Inner Loops

The inner loops do not solve a separate subproblem. Their job is to position each pointer on the next character that matters.

## Left Inner Loop

```python
while left < right and not s[left].isalnum():
    left += 1
```

English translation:

```text
While the pointers have not crossed and the left character is invalid,
keep moving the left pointer forward.
```

The loop stops when either:

- `left` reaches or crosses `right`, or
- `s[left]` is a valid letter or digit.

## Right Inner Loop

```python
while left < right and not s[right].isalnum():
    right -= 1
```

English translation:

```text
While the pointers have not crossed and the right character is invalid,
keep moving the right pointer backward.
```

The loop stops when either:

- `left` reaches or crosses `right`, or
- `s[right]` is a valid letter or digit.

---

# Why the Inner Loops Are Necessary

Consider:

```python
s = "A man, a plan, a canal: Panama"
```

The first comparison is:

```text
'A' <-> 'a'
```

After lowercase conversion, they match.

Both pointers move inward. The left pointer now lands on a space:

```text
 A man, a plan, a canal: Panama
  ^
 left
```

Without the left inner loop, the algorithm would compare:

```text
' ' <-> 'm'
```

and incorrectly return `False`.

The inner loop skips the space and moves `left` to `m`.

---

# Detailed Inner-Loop Example

Use this smaller input:

```python
s = "A, b:a"
```

Character positions:

```text
Index:  0 1 2 3 4 5
Char:   A , _ b : a
```

Here `_` represents a space.

Initially:

```python
left = 0
right = 5
```

## Outer Iteration 1

Pointers:

```text
A ,   b : a
^         ^
L         R
```

Both characters are alphanumeric, so the inner loops do not move.

Compare:

```python
'A'.lower() == 'a'.lower()
```

Result:

```python
True
```

Move both inward:

```python
left = 1
right = 4
```

## Outer Iteration 2: Left Inner Loop

Current characters:

```text
A ,   b : a
  ^     ^
  L     R
```

Left points to:

```python
','
```

Check:

```python
','.isalnum()
```

Result:

```python
False
```

Move left:

```python
left = 2
```

Left now points to a space.

Check:

```python
' '.isalnum()
```

Result:

```python
False
```

Move left again:

```python
left = 3
```

Left now points to:

```python
'b'
```

Check:

```python
'b'.isalnum()
```

Result:

```python
True
```

The left inner loop stops.

## Outer Iteration 2: Right Inner Loop

Right points to:

```python
':'
```

Check:

```python
':'.isalnum()
```

Result:

```python
False
```

Move right:

```python
right = 3
```

Now:

```python
left == right == 3
```

The right inner loop stops because `left < right` is no longer true.

The outer loop completes, and the method returns:

```python
True
```

The middle character `b` does not need a mirrored comparison.

---

# Movement Rules

This problem has four movement cases.

## Case 1: Left Character Is Invalid

```python
if not s[left].isalnum():
    left += 1
```

Move only the left pointer.

## Case 2: Right Character Is Invalid

```python
if not s[right].isalnum():
    right -= 1
```

Move only the right pointer.

## Case 3: Both Characters Are Valid and Match

```python
left += 1
right -= 1
```

Both mirrored positions have been validated.

## Case 4: Both Characters Are Valid and Differ

```python
return False
```

A single valid-character mismatch proves that the string is not a palindrome.

---

# Why `left < right` Appears in Every Loop

The condition:

```python
left < right
```

prevents the pointers from crossing while skipping invalid characters.

It also ensures that two distinct mirrored positions are compared.

For an odd-length palindrome:

```text
r a c e c a r
      ^
```

The pointers eventually meet at the middle character. A middle character has no separate mirror and does not need comparison.

---

# Why the Nested Loops Are Still O(n)

The code contains an outer loop and two inner loops, but the solution is not `O(n^2)`.

Why?

```text
left only moves forward
right only moves backward
```

Neither pointer resets.

Across the complete algorithm:

- `left` moves at most `n` positions.
- `right` moves at most `n` positions.

Total pointer movements are bounded by approximately `2n`.

```text
O(2n) = O(n)
```

Nested syntax does not automatically mean quadratic complexity. Repeated traversal of the same elements causes quadratic behavior. That does not happen here.

---

# Dry Run: Valid Palindrome

Input:

```python
s = "A man, a plan, a canal: Panama"
```

The meaningful comparisons are:

```text
A <-> a
m <-> m
a <-> a
n <-> n
a <-> a
p <-> P
l <-> l
a <-> a
n <-> n
a <-> a
c <-> c
a <-> a
n <-> n
a <-> a
l <-> l
```

Spaces and punctuation are skipped by the inner loops.

Every valid mirrored pair matches after lowercase conversion, so the method returns:

```python
True
```

---

# Dry Run: Invalid Palindrome

Input:

```python
s = "race a car"
```

After skipping spaces and comparing lowercase valid characters, the algorithm eventually finds a mismatch.

At that point:

```python
if s[left].lower() != s[right].lower():
    return False
```

The method stops immediately because one mismatch is enough.

---

# Complexity Analysis

Let `n` be the length of the string.

## Time Complexity

```text
O(n)
```

Each pointer moves across the string at most once.

## Space Complexity

```text
O(1)
```

The algorithm uses only two integer pointers and does not create a cleaned copy of the string.

> Calling `.lower()` produces small character strings during comparison, but the auxiliary storage does not grow with the input size.

---

# Loop Invariant

Before each outer-loop comparison:

```text
All meaningful mirrored character pairs outside the range [left, right]
have already been validated.
```

The inner loops then move both boundaries to the next meaningful characters.

This invariant explains why processed positions never need to be revisited.

---

# Alternative Control Flow Without Inner Loops

The same movement rules can be written with one loop and conditional branches:

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1

        while left < right:
            if not s[left].isalnum():
                left += 1
            elif not s[right].isalnum():
                right -= 1
            elif s[left].lower() != s[right].lower():
                return False
            else:
                left += 1
                right -= 1

        return True
```

This version is also correct.

## Difference Between the Versions

### Inner-Loop Version

```text
Skip all invalid characters first.
Then compare one valid pair.
```

### Branch Version

```text
Perform one pointer action per outer iteration.
```

The inner-loop version makes the phases explicit, while the branch version can be easier to trace one movement at a time.

Both use:

```text
Time: O(n)
Space: O(1)
```

---

# Common Mistakes

## Mistake 1: Comparing Before Skipping Invalid Characters

Incorrect order:

```python
if s[left].lower() != s[right].lower():
    return False
```

If either pointer is on punctuation or whitespace, the comparison is not meaningful.

Skip first, compare second.

## Mistake 2: Moving Both Pointers When Only One Is Invalid

If the left character is punctuation, move only `left`.

Moving `right` too could skip a valid character that still needs comparison.

## Mistake 3: Forgetting Lowercase Conversion

```python
'A' != 'a'
```

Use:

```python
s[left].lower() == s[right].lower()
```

## Mistake 4: Using `isalpha()` Instead of `isalnum()`

`isalpha()` accepts letters but rejects digits.

The problem considers both letters and numbers valid, so use:

```python
isalnum()
```

## Mistake 5: Using `left <= right` Unnecessarily

The middle character does not need comparison with itself. Use:

```python
while left < right:
```

## Mistake 6: Thinking the Inner Loops Make It O(n²)

The pointers never reset. Total movement remains linear.

## Mistake 7: Concatenating a Cleaned String Repeatedly

This approach:

```python
cleaned = ""

for char in s:
    if char.isalnum():
        cleaned += char.lower()
```

can involve repeated string creation because strings are immutable. If using the cleaned-string approach, append to a list and join once.

---

# Interview Explanation

A concise explanation:

> A palindrome compares mirrored characters, so I use one pointer at each end of the string. Before comparing, each pointer skips non-alphanumeric characters. I then compare the lowercase valid characters. A mismatch returns false immediately; otherwise, both pointers move inward. Each pointer moves in only one direction, so the solution takes O(n) time and O(1) auxiliary space.

---

# Reusable Template

```python
left = 0
right = len(items) - 1

while left < right:
    while left < right and left_item_is_invalid:
        left += 1

    while left < right and right_item_is_invalid:
        right -= 1

    if normalized(items[left]) != normalized(items[right]):
        return False

    left += 1
    right -= 1

return True
```

---

# Pattern Takeaway

LC 125 teaches the first major Two-Pointer pattern:

```text
Opposite-direction pointers with independent skipping rules
```

The most important learning is the movement logic:

```text
Invalid left  -> move left only
Invalid right -> move right only
Valid match   -> move both
Valid mismatch -> stop
```

The inner loops simply find the next meaningful character from each direction before comparison.

---

# Progress

```text
Two Pointers

[x] LC 125 - Valid Palindrome
[ ] LC 167 - Two Sum II
[ ] LC 283 - Move Zeroes
[ ] LC 11  - Container With Most Water
[ ] LC 15  - 3Sum
```
