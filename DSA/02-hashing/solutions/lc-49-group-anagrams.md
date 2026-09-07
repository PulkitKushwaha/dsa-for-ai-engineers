# LC 49 - Group Anagrams

## Problem Link

[Open LC 49 - Group Anagrams on LeetCode](https://leetcode.com/problems/group-anagrams/)

---

## Problem Statement

Given an array of strings `strs`, group the anagrams together.

An anagram is a word formed by rearranging the characters of another word while preserving the frequency of every character.

The groups may be returned in any order.

---

## Examples

### Example 1

```python
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
```

Possible output:

```python
[
    ["eat", "tea", "ate"],
    ["tan", "nat"],
    ["bat"]
]
```

Explanation:

```text
"eat", "tea", and "ate" contain the same characters with the same frequencies.
"tan" and "nat" contain the same characters with the same frequencies.
"bat" has no other matching anagram.
```

### Example 2

```python
strs = [""]
```

Output:

```python
[[""]]
```

### Example 3

```python
strs = ["a"]
```

Output:

```python
[["a"]]
```

---

# Pattern Recognition

## Signals

Look for:

```text
Group similar items
Group anagrams
Categorize words
Items with the same composition
Multiple values belong to one category
```

## Pattern

```text
Hashing -> Grouping by Signature
```

## Recognition Shortcut

Ask:

```text
What common key can represent all items that belong together?
```

For anagrams, all words in the same group have the same sorted representation.

```python
"eat" -> "aet"
"tea" -> "aet"
"ate" -> "aet"
```

Therefore:

```text
signature -> list of original words
```

## Mental Model

Imagine labeled buckets.

Each word creates a label called a signature. Words with the same label are placed in the same bucket.

```text
"aet" bucket -> ["eat", "tea", "ate"]
"ant" bucket -> ["tan", "nat"]
"abt" bucket -> ["bat"]
```

---

# Why Frequency Matters

Anagrams must contain the same characters with the same frequencies.

For example:

```python
"aab"
"aba"
"baa"
```

All three contain:

```text
a -> 2
b -> 1
```

Their sorted signature is identical:

```python
"aab"
```

A set would not be sufficient because it would lose duplicate counts.

```python
set("aab") == {'a', 'b'}
set("abb") == {'a', 'b'}
```

The sets are equal, but the words are not anagrams.

---

# Brute Force Approach

## Idea

Process each word and compare it with words that have not yet been grouped.

For every pair of words, determine whether they are anagrams by sorting or comparing frequency maps.

## Why It Is Expensive

If there are `n` words, pairwise comparison may require approximately:

```text
O(n^2)
```

comparisons.

If every comparison also sorts words of length `k`, the cost can approach:

```text
O(n^2 * k log k)
```

The repeated comparison can be avoided by calculating one signature per word and using a dictionary lookup.

---

# Key Observation

We do not need to compare every word with every other word.

We need a deterministic representation such that:

```text
Two words are anagrams if and only if their signatures are equal.
```

A simple signature is:

```python
"".join(sorted(word))
```

Example:

```python
word = "tea"
signature = "".join(sorted(word))
```

Result:

```python
"aet"
```

---

# Approach 1: Normal Dictionary

## Why Learn This Version First?

The normal dictionary version exposes the underlying grouping logic explicitly:

```text
If the bucket does not exist, create it.
Then append the current word to the bucket.
```

## Algorithm

1. Create an empty dictionary called `groups`.
2. Traverse every word.
3. Sort the word to create its signature.
4. If the signature is not already a key, create an empty list.
5. Append the original word to the signature's list.
6. Return all dictionary values.

## Solution Without `defaultdict`

```python
class Solution:
    def groupAnagrams(self, strs):
        groups = {}

        for word in strs:
            signature = "".join(sorted(word))

            if signature not in groups:
                groups[signature] = []

            groups[signature].append(word)

        return list(groups.values())
```

---

# Understanding the Dictionary

The dictionary does not store:

```text
word -> frequency
```

It stores:

```text
signature -> list of words
```

Example:

```python
{
    "aet": ["eat", "tea", "ate"],
    "ant": ["tan", "nat"],
    "abt": ["bat"]
}
```

The values are lists because multiple words may belong to the same signature.

---

# Dry Run

Input:

```python
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
```

Initially:

```python
groups = {}
```

## Process `"eat"`

Signature:

```python
"aet"
```

The key does not exist, so create its bucket and append the word:

```python
{
    "aet": ["eat"]
}
```

## Process `"tea"`

Signature:

```python
"aet"
```

The bucket already exists:

```python
{
    "aet": ["eat", "tea"]
}
```

## Process `"tan"`

Signature:

```python
"ant"
```

Create a new bucket:

```python
{
    "aet": ["eat", "tea"],
    "ant": ["tan"]
}
```

## Process `"ate"`

Signature:

```python
"aet"
```

Append to the existing bucket:

```python
{
    "aet": ["eat", "tea", "ate"],
    "ant": ["tan"]
}
```

## Process `"nat"`

Signature:

```python
"ant"
```

Append:

```python
{
    "aet": ["eat", "tea", "ate"],
    "ant": ["tan", "nat"]
}
```

## Process `"bat"`

Signature:

```python
"abt"
```

Final dictionary:

```python
{
    "aet": ["eat", "tea", "ate"],
    "ant": ["tan", "nat"],
    "abt": ["bat"]
}
```

Return:

```python
list(groups.values())
```

Possible result:

```python
[
    ["eat", "tea", "ate"],
    ["tan", "nat"],
    ["bat"]
]
```

---

# Approach 2: Using `defaultdict(list)`

## What `defaultdict` Changes

With a normal dictionary, a new bucket must be initialized manually:

```python
if signature not in groups:
    groups[signature] = []
```

A `defaultdict(list)` automatically creates an empty list when a missing key is accessed.

## Solution

```python
from collections import defaultdict


class Solution:
    def groupAnagrams(self, strs):
        groups = defaultdict(list)

        for word in strs:
            signature = "".join(sorted(word))
            groups[signature].append(word)

        return list(groups.values())
```

## Important Learning

`defaultdict` is not a different algorithm.

Both versions implement:

```text
signature -> list of words
```

The only difference is bucket initialization.

```text
Normal dict:
Create missing list manually.

Defaultdict:
Create missing list automatically.
```

---

# Complexity Analysis

Let:

```text
n = number of words
k = maximum or average word length
```

## Time Complexity

Sorting one word takes:

```text
O(k log k)
```

Doing this for `n` words takes:

```text
O(n * k log k)
```

Dictionary insertion and lookup are average `O(1)` per word, excluding signature construction.

Therefore, total time is:

```text
O(n * k log k)
```

## Space Complexity

The output and grouping dictionary store all characters from all words:

```text
O(n * k)
```

Sorted signatures also require space. The exact temporary-space accounting depends on implementation, but the overall storage grows with the total input size.

---

# Alternative Signature: Character Frequency Tuple

Sorting is easy to understand, but the problem can also be solved using a frequency signature.

If input words contain only lowercase English letters, create a list of 26 counts.

Example:

```python
"eat"
```

produces counts where:

```text
a -> 1
e -> 1
t -> 1
```

Convert the list to a tuple because dictionary keys must be hashable.

## Solution

```python
from collections import defaultdict


class Solution:
    def groupAnagrams(self, strs):
        groups = defaultdict(list)

        for word in strs:
            counts = [0] * 26

            for char in word:
                index = ord(char) - ord('a')
                counts[index] += 1

            signature = tuple(counts)
            groups[signature].append(word)

        return list(groups.values())
```

## Complexity

Creating a frequency signature requires one traversal of each word:

```text
O(k)
```

Across all words:

```text
O(n * k)
```

This avoids sorting but is less immediately intuitive and assumes a known fixed alphabet.

## Which Version Should Be Learned First?

Start with the sorted-signature version because it makes the grouping insight obvious.

Then recognize the frequency tuple as an optimization when:

- The alphabet is fixed.
- Sorting cost matters.
- The interviewer asks for an alternative approach.

---

# Why a List Becomes the Dictionary Value

A signature may correspond to multiple original words.

For example:

```python
"aet"
```

belongs to:

```python
["eat", "tea", "ate"]
```

Therefore, the mapping must be:

```text
signature -> list of words
```

not:

```text
signature -> one word
```

If only one word were stored, later words would overwrite earlier ones.

---

# Why We Store the Original Word

The signature is only used to identify the group.

The required output contains the original words, not their sorted forms.

Therefore:

```python
groups[signature].append(word)
```

not:

```python
groups[signature].append(signature)
```

---

# Common Mistakes

## Mistake 1: Using a Set as the Signature

A set loses character frequencies.

```python
set("aab") == set("abb")
```

Both become:

```python
{'a', 'b'}
```

But the words are not anagrams.

## Mistake 2: Using `sorted(word)` Directly as a Dictionary Key

```python
sorted(word)
```

returns a list.

Lists are mutable and cannot be dictionary keys.

Incorrect:

```python
groups[sorted(word)].append(word)
```

Convert the sorted characters to an immutable string or tuple:

```python
signature = "".join(sorted(word))
```

or:

```python
signature = tuple(sorted(word))
```

## Mistake 3: Forgetting to Initialize a Normal Dictionary Bucket

Incorrect:

```python
groups = {}
groups[signature].append(word)
```

This raises a `KeyError` for a new signature.

Correct:

```python
if signature not in groups:
    groups[signature] = []

groups[signature].append(word)
```

## Mistake 4: Storing Only One Word per Signature

Incorrect:

```python
groups[signature] = word
```

A later anagram overwrites the earlier one.

Correct:

```python
groups[signature].append(word)
```

## Mistake 5: Returning the Dictionary

The expected result is a list of groups.

Return:

```python
list(groups.values())
```

not:

```python
groups
```

## Mistake 6: Sorting the Entire Input List Instead of Each Word

The signature must describe the characters inside each individual word.

Correct:

```python
for word in strs:
    signature = "".join(sorted(word))
```

## Mistake 7: Treating This as Only a Frequency-Counting Problem

Frequency is part of defining an anagram, but the output requirement is grouping.

The core mapping is:

```text
signature -> group members
```

---

# Interview Explanation

A concise explanation:

> Anagrams have the same characters with the same frequencies, so sorting each word produces the same canonical signature for all words in an anagram group. I will use that signature as a dictionary key and store the original words in a list under that key. After processing every word, I return the dictionary values. With sorting, the time complexity is O(n * k log k), where n is the number of words and k is the word length.

---

# Reusable Grouping Template

## Normal Dictionary

```python
groups = {}

for item in items:
    signature = create_signature(item)

    if signature not in groups:
        groups[signature] = []

    groups[signature].append(item)

return list(groups.values())
```

## `defaultdict`

```python
from collections import defaultdict

groups = defaultdict(list)

for item in items:
    signature = create_signature(item)
    groups[signature].append(item)

return list(groups.values())
```

---

# Pattern Comparison

## LC 242 - Valid Anagram

Question:

```text
Do two words have identical character frequencies?
```

Mapping:

```text
character -> frequency
```

## LC 1 - Two Sum

Question:

```text
Have I already seen the required complement?
```

Mapping:

```text
number -> index
```

## LC 219 - Contains Duplicate II

Question:

```text
Where was this value last seen?
```

Mapping:

```text
value -> last index
```

## LC 49 - Group Anagrams

Question:

```text
Which words share the same signature?
```

Mapping:

```text
signature -> list of words
```

---

# Similar Problems

- LC 242 - Valid Anagram
- LC 347 - Top K Frequent Elements
- LC 451 - Sort Characters by Frequency
- LC 692 - Top K Frequent Words
- LC 249 - Group Shifted Strings

---

# What to Memorize

Do not memorize the complete implementation. Memorize this chain:

```text
Signal:
Group / categorize / similar composition / anagrams

Question:
What common key identifies items that belong together?

Pattern:
Hashing -> Grouping by Signature

Signature:
Sorted word or character-frequency tuple

Structure:
Dictionary or defaultdict(list)

Mapping:
signature -> list of original words

Sorted-signature time:
O(n * k log k)
```

---

# Pattern Takeaway

LC 49 introduces the core HashMap grouping pattern:

```text
Create a canonical signature.
Use the signature as a dictionary key.
Append every matching item to the same bucket.
```

The specific signature changes between problems, but the reusable idea remains:

```text
same signature -> same group
```

---

# Progress

- [x] LC 217 - Contains Duplicate
- [x] LC 242 - Valid Anagram
- [x] LC 387 - First Unique Character in a String
- [x] LC 383 - Ransom Note
- [x] LC 1 - Two Sum
- [x] LC 219 - Contains Duplicate II
- [x] LC 49 - Group Anagrams
- [ ] LC 347 - Top K Frequent Elements
- [ ] LC 202 - Happy Number
- [ ] LC 128 - Longest Consecutive Sequence
