# LC 242 - Valid Anagram

## Problem Link

[Open LC 242 - Valid Anagram on LeetCode](https://leetcode.com/problems/valid-anagram/)

---

## Problem Statement

Given two strings, `s` and `t`, return `True` if `t` is an anagram of `s`. Otherwise, return `False`.

An **anagram** is formed by rearranging all characters of another string while preserving the frequency of every character.

---

## Examples

### Example 1

```python
s = "anagram"
t = "nagaram"
```

Output:

```python
True
```

Both strings contain the same characters with the same frequencies.

### Example 2

```python
s = "rat"
t = "car"
```

Output:

```python
False
```

Their character frequencies are different.

---

## Pattern Recognition

### Signals

- Anagram
- Same characters
- Same occurrences
- Character counts
- Frequency comparison
- Order does not matter

### Pattern

```text
Hashing -> Frequency Counting
```

### Recognition Shortcut

When the order of elements does not matter, but the number of occurrences does, think:

```python
Counter
```

or:

```python
dict
```

### Mental Question

```text
Do both inputs contain each item the same number of times?
```

---

## Important Early Check

If the two strings have different lengths, they cannot be anagrams.

```python
if len(s) != len(t):
    return False
```

This check is not required when directly comparing two `Counter` objects, because unequal counts will already produce `False`. However, it is useful when manually building a single frequency dictionary.

---

# Approach 1: Sorting

## Idea

If two strings are anagrams, sorting their characters should produce identical sequences.

```python
sorted("anagram") == sorted("nagaram")
```

Both become:

```python
['a', 'a', 'a', 'g', 'm', 'n', 'r']
```

## Solution

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)
```

## Complexity Analysis

Let `n` be the length of `s` and `m` be the length of `t`.

### Time Complexity

```text
O(n log n + m log m)
```

If both strings have the same length, this is commonly simplified to:

```text
O(n log n)
```

### Space Complexity

```text
O(n + m)
```

Python's `sorted()` creates new lists containing the sorted characters.

## Assessment

This is simple and readable, but it does more work than necessary because the problem only requires frequency comparison, not ordering.

---

# Approach 2: Frequency Counting with `Counter`

## Key Observation

Two strings are anagrams if and only if their character-frequency maps are equal.

For:

```python
s = "anagram"
```

The frequency map is:

```python
{
    'a': 3,
    'n': 1,
    'g': 1,
    'r': 1,
    'm': 1
}
```

The string `"nagaram"` produces the same map.

## Optimal Python Solution

```python
from collections import Counter


class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return Counter(s) == Counter(t)
```

## How It Works

1. `Counter(s)` counts every character in `s`.
2. `Counter(t)` counts every character in `t`.
3. Equality checks whether both counters contain identical keys and counts.

## Dry Run

Input:

```python
s = "anagram"
t = "nagaram"
```

First counter:

```python
Counter({
    'a': 3,
    'n': 1,
    'g': 1,
    'r': 1,
    'm': 1
})
```

Second counter:

```python
Counter({
    'a': 3,
    'n': 1,
    'g': 1,
    'r': 1,
    'm': 1
})
```

Comparison:

```python
True
```

## Complexity Analysis

### Time Complexity

```text
O(n + m)
```

Each string is traversed to build its frequency map.

If both strings have equal length, this is commonly simplified to:

```text
O(n)
```

### Space Complexity

```text
O(k)
```

Here, `k` is the number of distinct characters stored in the frequency maps.

For a fixed lowercase English alphabet, `k <= 26`, so auxiliary space may be described as:

```text
O(1)
```

For unrestricted Unicode characters, use the more general:

```text
O(k)
```

---

# Approach 3: Manual Frequency Dictionary

## Why Learn This Version?

The `Counter` solution is concise, but interviewers may ask you to implement the frequency logic manually. This version exposes the underlying hashing pattern.

## Idea

1. If the lengths differ, return `False`.
2. Count every character from `s`.
3. Subtract the count for every character from `t`.
4. If a required character is missing or its count becomes negative, return `False`.
5. Otherwise, return `True`.

## Solution

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        frequency = {}

        for char in s:
            frequency[char] = frequency.get(char, 0) + 1

        for char in t:
            if char not in frequency:
                return False

            frequency[char] -= 1

            if frequency[char] < 0:
                return False

        return True
```

## Alternative Manual Version

Count both strings inside one loop:

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        count_s = {}
        count_t = {}

        for index in range(len(s)):
            count_s[s[index]] = count_s.get(s[index], 0) + 1
            count_t[t[index]] = count_t.get(t[index], 0) + 1

        return count_s == count_t
```

## Complexity Analysis

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(k)
```

For a fixed lowercase English alphabet, this can be treated as `O(1)` auxiliary space.

---

# Why a Set Is Not Enough

A set only records whether a character exists. It does not preserve its frequency.

Consider:

```python
s = "aab"
t = "abb"
```

Both produce the same set:

```python
{'a', 'b'}
```

But they are not anagrams:

```text
s: a = 2, b = 1
t: a = 1, b = 2
```

Therefore:

```text
Existence only -> set
Frequency required -> Counter or dict
```

---

# Common Mistakes

## Mistake 1: Comparing Sets

Incorrect:

```python
return set(s) == set(t)
```

This ignores duplicate counts.

## Mistake 2: Checking Only Character Existence

Finding every character from `s` somewhere in `t` is insufficient. The counts must also match.

## Mistake 3: Forgetting the Length Constraint

In a manual implementation, checking unequal lengths early keeps the logic simple and prevents false positives.

## Mistake 4: Claiming Sorting Is `O(n)`

Sorting is generally:

```text
O(n log n)
```

Frequency counting is:

```text
O(n)
```

## Mistake 5: Always Calling the Space Complexity `O(1)`

`O(1)` is appropriate only when the character set is fixed, such as 26 lowercase English letters. For an unrestricted character set, describe the space as `O(k)`, where `k` is the number of distinct characters.

## Mistake 6: Memorizing the One-Line Solution Without the Pattern

The important learning is not:

```python
Counter(s) == Counter(t)
```

The important learning is:

```text
Order is irrelevant + frequencies must match -> Frequency Map
```

---

# Interview Explanation

A concise explanation you can give during an interview:

> The order of characters does not matter for an anagram, but the frequency of each character must match. I will build a frequency map for both strings and compare them. In Python, `Counter` represents this directly. Building the counters takes linear time, and the extra space depends on the number of distinct characters.

---

# Pattern Template

## Compare Two Frequency Maps

```python
from collections import Counter


def have_same_frequencies(first, second):
    return Counter(first) == Counter(second)
```

## Manual Frequency Map

```python
frequency = {}

for item in items:
    frequency[item] = frequency.get(item, 0) + 1
```

## Count First, Search Second

```python
from collections import Counter

frequency = Counter(items)

for item in items:
    if frequency[item] == required_count:
        return item
```

The final template prepares us for LC 387, where we will count characters first and then search for the first character with frequency `1`.

---

# Similar Problems

- LC 383 - Ransom Note
- LC 387 - First Unique Character in a String
- LC 49 - Group Anagrams
- LC 438 - Find All Anagrams in a String
- LC 567 - Permutation in String

The final two combine frequency counting with the Sliding Window pattern and will be revisited later in the roadmap.

---

# What to Memorize

```text
Signal:
Anagram / same occurrences / order does not matter

Question:
Does every item appear the same number of times?

Pattern:
Hashing -> Frequency Counting

Tool:
Counter or dict

Preferred Python Solution:
Counter(s) == Counter(t)

Time:
O(n + m)

Space:
O(k), or O(1) for a fixed-size alphabet
```

---

# Pattern Takeaway

This problem introduces the second major hashing reflex:

```text
Duplicate or seen before -> Set
Frequency or occurrence count -> Counter / Dictionary
```

The code is short, but the transferable lesson is recognizing when order is irrelevant and frequency is the real information that must be preserved.

---

# Progress

- [x] LC 217 - Contains Duplicate
- [x] LC 242 - Valid Anagram
- [ ] LC 387 - First Unique Character in a String
