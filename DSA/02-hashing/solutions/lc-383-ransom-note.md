# LC 383 - Ransom Note

## Problem Link

[Open LC 383 - Ransom Note on LeetCode](https://leetcode.com/problems/ransom-note/)

---

## Problem Statement

Given two strings, `ransomNote` and `magazine`, return `True` if `ransomNote` can be constructed using the letters from `magazine`.

Each character from `magazine` can be used only once.

Otherwise, return `False`.

---

## Examples

### Example 1

```python
ransomNote = "a"
magazine = "b"
```

Output:

```python
False
```

The required character `a` is not available in `magazine`.

### Example 2

```python
ransomNote = "aa"
magazine = "ab"
```

Output:

```python
False
```

The ransom note requires two copies of `a`, but the magazine contains only one.

### Example 3

```python
ransomNote = "aa"
magazine = "aab"
```

Output:

```python
True
```

The magazine contains both copies of `a` required by the ransom note.

---

## Pattern Recognition

### Signals

- Can construct
- Can form
- Available characters
- Each item can be used once
- Enough copies
- Required frequency versus available frequency

### Pattern

```text
Hashing -> Frequency Counting -> Inventory Consumption
```

### Recognition Shortcut

When a problem asks whether one collection can be built from another collection, think:

```text
Available collection -> Inventory
Required collection  -> Demand
```

Then ask:

```text
Does the inventory contain enough copies of every required item?
```

### Mental Model

Treat `magazine` as a warehouse.

```text
Build inventory from magazine
Consume inventory for ransomNote
Fail if any required character is unavailable
```

---

## Important Early Check

If `ransomNote` is longer than `magazine`, construction is impossible.

```python
if len(ransomNote) > len(magazine):
    return False
```

This check is optional because the frequency solution will still return the correct answer. However, it can reject an impossible case immediately.

---

# Brute Force Approach

## Idea

Convert the magazine into a mutable list. For every character required by `ransomNote`:

1. Search for the character in the list.
2. If it does not exist, return `False`.
3. If it exists, remove one copy so that it cannot be reused.

## Solution

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        available = list(magazine)

        for char in ransomNote:
            if char not in available:
                return False

            available.remove(char)

        return True
```

## Complexity Analysis

Let:

```text
n = length of ransomNote
m = length of magazine
```

### Time Complexity

Both membership checking and removal from a list can take linear time.

```text
O(n * m)
```

### Space Complexity

```text
O(m)
```

The magazine is copied into a list.

## Why It Is Not Optimal

The available characters are searched repeatedly. A frequency dictionary lets us build the inventory once and perform average `O(1)` lookups afterward.

---

# Optimal Approach: Manual Frequency Dictionary

## Key Observation

The problem does not only ask whether a character exists.

It asks whether enough copies of the character exist.

For example:

```python
ransomNote = "aa"
magazine = "ab"
```

A set would show that `a` exists, but it would not show that only one copy is available.

Therefore, we need a frequency map.

---

## Algorithm

1. Optionally reject the case where `ransomNote` is longer than `magazine`.
2. Create an empty dictionary called `inventory`.
3. Count every character in `magazine`.
4. Traverse `ransomNote`.
5. For each required character:
   - If its available count is zero, return `False`.
   - Otherwise, consume one copy by decrementing its count.
6. If every character is successfully consumed, return `True`.

---

## Optimal Solution Without `Counter`

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        if len(ransomNote) > len(magazine):
            return False

        inventory = {}

        for char in magazine:
            inventory[char] = inventory.get(char, 0) + 1

        for char in ransomNote:
            if inventory.get(char, 0) == 0:
                return False

            inventory[char] -= 1

        return True
```

---

## Why `inventory.get(char, 0)` Is Useful

This expression:

```python
inventory.get(char, 0)
```

returns:

- The current count if `char` exists in the dictionary.
- `0` if `char` does not exist.

Therefore, this one condition handles two failure cases:

```python
if inventory.get(char, 0) == 0:
    return False
```

### Failure Case 1: Character Never Existed

```python
inventory = {'a': 1}
required = 'b'
```

Result:

```python
inventory.get('b', 0) == 0
```

### Failure Case 2: All Copies Were Already Consumed

```python
inventory = {'a': 0}
required = 'a'
```

Result:

```python
inventory.get('a', 0) == 0
```

---

# Dry Run

Input:

```python
ransomNote = "aa"
magazine = "aab"
```

## Step 1: Build Inventory

After traversing `magazine`:

```python
{
    'a': 2,
    'b': 1
}
```

## Step 2: Consume Required Characters

First required character:

```python
char = 'a'
inventory['a'] = 2
```

One copy is available, so consume it:

```python
inventory['a'] = 1
```

Second required character:

```python
char = 'a'
inventory['a'] = 1
```

Consume it:

```python
inventory['a'] = 0
```

All required characters were supplied.

Return:

```python
True
```

---

# Failure Dry Run

Input:

```python
ransomNote = "aa"
magazine = "ab"
```

Initial inventory:

```python
{
    'a': 1,
    'b': 1
}
```

Consume first `a`:

```python
inventory['a'] = 0
```

Request second `a`:

```python
inventory.get('a', 0) == 0
```

No copy remains, so return:

```python
False
```

---

# Complexity Analysis

Let:

```text
n = length of ransomNote
m = length of magazine
k = number of distinct characters in magazine
```

## Time Complexity

Building the inventory:

```text
O(m)
```

Processing the ransom note:

```text
O(n)
```

Total:

```text
O(n + m)
```

## Space Complexity

```text
O(k)
```

The dictionary stores one entry per distinct magazine character.

If the input is restricted to 26 lowercase English letters, the map contains at most 26 entries. Under that fixed-alphabet assumption, auxiliary space can be described as:

```text
O(1)
```

For a general character set, use:

```text
O(k)
```

---

# Alternative Approach Using `Counter`

```python
from collections import Counter


class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        inventory = Counter(magazine)

        for char in ransomNote:
            if inventory[char] == 0:
                return False

            inventory[char] -= 1

        return True
```

This uses the same inventory-consumption algorithm. `Counter` only creates the frequency map more concisely.

---

# Alternative Counter Comparison

Another valid Python approach is to compare required and available frequencies:

```python
from collections import Counter


class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        required = Counter(ransomNote)
        available = Counter(magazine)

        for char, required_count in required.items():
            if available[char] < required_count:
                return False

        return True
```

This expresses the problem as:

```text
available count >= required count
```

The manual inventory-consumption version is especially useful for learning the underlying pattern.

---

# Why a Set Is Not Enough

Consider:

```python
ransomNote = "aa"
magazine = "ab"
```

The magazine set is:

```python
{'a', 'b'}
```

The set confirms that `a` exists but loses the fact that only one copy exists.

The actual requirement is:

```text
required a = 2
available a = 1
```

Therefore:

```text
Need existence only -> set
Need quantity        -> dictionary or Counter
```

---

# Common Mistakes

## Mistake 1: Checking Only Membership

Incorrect:

```python
for char in ransomNote:
    if char not in magazine:
        return False
```

This does not account for how many times a character is available.

## Mistake 2: Using a Set

A set discards duplicate counts, which are essential in this problem.

## Mistake 3: Forgetting to Decrement Inventory

Incorrect:

```python
if inventory.get(char, 0) > 0:
    continue
```

Each magazine character may be used only once. Successful use must consume one copy:

```python
inventory[char] -= 1
```

## Mistake 4: Direct Dictionary Access for a Missing Character

This can raise a `KeyError`:

```python
if inventory[char] == 0:
```

when `char` was never present in `magazine`.

Safer manual-dictionary check:

```python
if inventory.get(char, 0) == 0:
```

## Mistake 5: Building Inventory from the Wrong String

The inventory must be created from:

```python
magazine
```

because that is the source of available characters.

The demand comes from:

```python
ransomNote
```

## Mistake 6: Returning `True` Too Early

Return `True` only after every required character has been processed successfully.

---

# Interview Explanation

A concise explanation:

> I will treat the magazine as an inventory. First, I count how many copies of each character are available. Then I traverse the ransom note and consume one copy of every required character. If a character is missing or its count has already reached zero, I return false. Otherwise, all required characters can be supplied. The solution takes O(n + m) time and O(k) extra space.

---

# Reusable Template

## Build and Consume Inventory

```python
inventory = {}

for item in available_items:
    inventory[item] = inventory.get(item, 0) + 1

for item in required_items:
    if inventory.get(item, 0) == 0:
        return False

    inventory[item] -= 1

return True
```

This template applies whenever:

- Items are available in limited quantities.
- Each item can be used once.
- A target must be constructed from those items.

---

# Pattern Comparison

## LC 217 - Contains Duplicate

Question:

```text
Have I seen this value before?
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

## LC 383 - Ransom Note

Question:

```text
Does the available inventory satisfy all required frequencies?
```

Technique:

```text
Build inventory, then consume it
```

---

# Similar Problems

- LC 242 - Valid Anagram
- LC 387 - First Unique Character in a String
- LC 1160 - Find Words That Can Be Formed by Characters
- LC 1400 - Construct K Palindrome Strings
- LC 49 - Group Anagrams

---

# What to Memorize

Do not memorize the exact code. Memorize this chain:

```text
Signal:
Construct / form / build / use each item once / enough copies

Question:
Does available inventory satisfy required demand?

Pattern:
Hashing -> Frequency Counting

Technique:
Build inventory -> Validate demand -> Consume inventory

Tool:
Dictionary or Counter

Time:
O(n + m)

Space:
O(k), or O(1) for a fixed-size alphabet
```

---

# Pattern Takeaway

LC 383 introduces a reusable interpretation of frequency maps:

```text
A frequency map can represent inventory.
```

This extends hashing beyond simple counting. The counts now change as resources are consumed.

```text
Available count > 0 -> consume one copy
Available count == 0 -> requirement cannot be satisfied
```

---

# Progress

- [x] LC 217 - Contains Duplicate
- [x] LC 242 - Valid Anagram
- [x] LC 387 - First Unique Character in a String
- [x] LC 383 - Ransom Note
- [ ] LC 1 - Two Sum
