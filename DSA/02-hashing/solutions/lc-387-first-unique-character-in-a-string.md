# LC 387 - First Unique Character in a String

## Problem Link

[Open LC 387 - First Unique Character in a String on LeetCode](https://leetcode.com/problems/first-unique-character-in-a-string/)

---

## Problem Statement

Given a string `s`, return the index of the first non-repeating character.

If no such character exists, return:

```python
-1
```

---

## Examples

### Example 1

```python
s = "leetcode"
```

Output:

```python
0
```

Explanation:

The character `l` appears exactly once and is the first unique character.

### Example 2

```python
s = "loveleetcode"
```

Output:

```python
2
```

Explanation:

The characters at indices `0` and `1` repeat, while `v` at index `2` appears exactly once.

### Example 3

```python
s = "aabb"
```

Output:

```python
-1
```

Explanation:

Every character repeats, so no unique character exists.

---

## Pattern Recognition

### Signals

- First unique character
- First non-repeating character
- Appears exactly once
- Frequency of characters
- Preserve original order

### Pattern

```text
Hashing -> Frequency Counting -> Two Passes
```

### Recognition Shortcut

When a problem asks for the first item satisfying a frequency condition, think:

```text
Pass 1: Count every item
Pass 2: Traverse the original order and find the answer
```

### Mental Question

```text
How often does each character appear, and which character with frequency 1 appears first?
```

---

## Why One Pass Is Not Straightforward

Consider:

```python
s = "aab"
```

When the first `a` is encountered, it temporarily looks unique. However, another `a` appears later.

Therefore, we should first collect complete frequency information and then determine which unique character appears first.

---

# Brute Force Approach

## Idea

For every character, scan the complete string and count how many times it occurs.

Return the index of the first character whose count is `1`.

## Solution

```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        for index in range(len(s)):
            count = 0

            for char in s:
                if char == s[index]:
                    count += 1

            if count == 1:
                return index

        return -1
```

## Complexity Analysis

### Time Complexity

```text
O(n^2)
```

For each character, the complete string may be scanned again.

### Space Complexity

```text
O(1)
```

No additional structure grows with the input.

## Why It Is Not Optimal

The same characters are counted repeatedly. A frequency dictionary lets us calculate all counts once and reuse them.

---

# Optimal Approach: Manual Frequency Dictionary

## Key Observation

The answer must satisfy two conditions:

1. Its frequency must be exactly `1`.
2. It must appear before every other character whose frequency is also `1`.

A dictionary handles the first condition, while traversing the original string handles the second.

---

## Algorithm

1. Create an empty dictionary named `frequency`.
2. Traverse the string and count every character.
3. Traverse the original string again using `enumerate()`.
4. Return the first index whose character has frequency `1`.
5. If no such character exists, return `-1`.

---

## Optimal Solution Without `Counter`

```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        frequency = {}

        for char in s:
            frequency[char] = frequency.get(char, 0) + 1

        for index, char in enumerate(s):
            if frequency[char] == 1:
                return index

        return -1
```

---

## Understanding `dict.get()`

This line:

```python
frequency[char] = frequency.get(char, 0) + 1
```

means:

1. Read the current count of `char`.
2. If `char` is not present, use `0`.
3. Add `1`.
4. Store the updated count.

It is equivalent to:

```python
if char not in frequency:
    frequency[char] = 0

frequency[char] += 1
```

The `get()` version is more concise, but both implement the same hashing idea.

---

# Dry Run

Input:

```python
s = "loveleetcode"
```

## Pass 1: Build the Frequency Map

After processing all characters:

```python
{
    'l': 2,
    'o': 2,
    'v': 1,
    'e': 4,
    't': 1,
    'c': 1,
    'd': 1
}
```

## Pass 2: Preserve Original Order

### Index 0

```python
char = 'l'
frequency['l'] = 2
```

Not unique, so continue.

### Index 1

```python
char = 'o'
frequency['o'] = 2
```

Not unique, so continue.

### Index 2

```python
char = 'v'
frequency['v'] = 1
```

This is the first unique character, so return:

```python
2
```

---

# Complexity Analysis

Let `n` be the length of the string and `k` be the number of distinct characters.

## Time Complexity

First traversal:

```text
O(n)
```

Second traversal:

```text
O(n)
```

Total:

```text
O(n)
```

Two separate linear traversals are still linear:

```text
O(n) + O(n) = O(2n) = O(n)
```

## Space Complexity

```text
O(k)
```

The dictionary stores one entry per distinct character.

If the input is restricted to lowercase English letters, at most 26 entries are stored. Under that fixed-alphabet assumption, auxiliary space can be described as:

```text
O(1)
```

For a general character set, use:

```text
O(k)
```

---

# Alternative Approach Using `Counter`

Python provides `Counter` as a convenient frequency-map utility.

```python
from collections import Counter


class Solution:
    def firstUniqChar(self, s: str) -> int:
        frequency = Counter(s)

        for index, char in enumerate(s):
            if frequency[char] == 1:
                return index

        return -1
```

The manual dictionary and `Counter` versions use the same algorithmic pattern and have the same asymptotic complexity.

```text
Counter is a Python convenience.
Frequency counting with hashing is the actual DSA concept.
```

---

# Why We Traverse the String Again

It may seem possible to iterate over the frequency dictionary and return a character with count `1`.

However, the problem asks for the first unique character according to its position in the original string.

The safest and clearest strategy is:

```text
Count using the dictionary
Search using the original string
```

The dictionary answers:

```text
How many times does this character occur?
```

The original string answers:

```text
Which qualifying character appears first?
```

---

# Why a Set Is Not Enough

A set can tell us whether a character exists, but it cannot tell us how many times it occurs.

For example:

```python
s = "aab"
```

The set is:

```python
{'a', 'b'}
```

It does not capture:

```text
a -> 2
b -> 1
```

Therefore:

```text
Existence only -> set
Frequency required -> dictionary or Counter
```

---

# Common Mistakes

## Mistake 1: Using `s.count()` Inside a Loop

```python
for index, char in enumerate(s):
    if s.count(char) == 1:
        return index
```

This looks concise, but `s.count(char)` scans the string each time. In the worst case, the total time becomes:

```text
O(n^2)
```

## Mistake 2: Returning a Character Instead of Its Index

The problem asks for:

```python
index
```

not:

```python
character
```

Use `enumerate()` to access both.

## Mistake 3: Returning Any Unique Character

The problem asks for the first unique character. Preserve the original order during the second traversal.

## Mistake 4: Forgetting the `-1` Case

If every character repeats, the method must return:

```python
-1
```

## Mistake 5: Using a Set

A set loses frequency information and cannot distinguish a character appearing once from one appearing multiple times.

## Mistake 6: Calling Two Passes `O(2n)` as the Final Complexity

`O(2n)` simplifies to:

```text
O(n)
```

Big-O notation ignores constant multipliers.

---

# Interview Explanation

A concise explanation:

> I need both the frequency of every character and the original order. I will first build a frequency dictionary in one pass. Then I will traverse the string again and return the first index whose character has frequency one. This takes O(n) time and O(k) extra space, where k is the number of distinct characters.

---

# Reusable Template

## Count First, Search Second

```python
frequency = {}

for item in items:
    frequency[item] = frequency.get(item, 0) + 1

for index, item in enumerate(items):
    if frequency[item] == required_frequency:
        return index

return not_found_value
```

For this problem:

```python
required_frequency = 1
not_found_value = -1
```

---

# Pattern Comparison

## LC 217 - Contains Duplicate

Question:

```text
Have I seen this before?
```

Tool:

```python
set
```

## LC 242 - Valid Anagram

Question:

```text
Do two inputs have identical frequencies?
```

Tool:

```python
dict or Counter
```

## LC 387 - First Unique Character

Question:

```text
Which first item has frequency 1?
```

Technique:

```text
Count first, search second
```

---

# Similar Problems

- LC 242 - Valid Anagram
- LC 383 - Ransom Note
- LC 451 - Sort Characters by Frequency
- LC 49 - Group Anagrams
- LC 387 - First Unique Character in a String

---

# What to Memorize

Do not memorize the exact implementation. Memorize this chain:

```text
Signal:
First unique / first non-repeating / appears once

Question:
How many times does every character appear?

Pattern:
Hashing -> Frequency Counting

Technique:
Pass 1 -> Count
Pass 2 -> Find the first qualifying element

Tool:
Dictionary or Counter

Time:
O(n)

Space:
O(k), or O(1) for a fixed-size alphabet
```

---

# Pattern Takeaway

This problem introduces a highly reusable two-pass pattern:

```text
Pass 1: Gather global information
Pass 2: Use that information while preserving original order
```

The important skill is recognizing that the answer cannot be confirmed until the complete frequency information is available.

---

# Progress

- [x] LC 217 - Contains Duplicate
- [x] LC 242 - Valid Anagram
- [x] LC 387 - First Unique Character in a String
- [ ] LC 383 - Ransom Note
