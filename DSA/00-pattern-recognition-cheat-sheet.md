# DSA Pattern Recognition Cheat Sheet

> The biggest challenge in Data Structures & Algorithms is not writing code.
>
> The real challenge is recognizing which algorithmic pattern a problem belongs to.
>
> This file serves as a mental map for identifying patterns quickly during LeetCode practice and interviews.

---

# Why Most People Struggle with DSA

Most people learn:

Arrays → Linked Lists → Trees → Graphs → Dynamic Programming

Then they open LeetCode and see:

"Longest Substring Without Repeating Characters"

And suddenly have no idea where to start.

Why?

Because interview problems are rarely labeled.

Nobody tells you:

> "Please use Sliding Window for this problem."

Instead, you must recognize the pattern yourself.

---

# The Correct Mental Model

Do NOT think:

Question → Code

Do NOT think:

Question → Data Structure

Think:

Question → Signals → Pattern → Template → Code

Example:

Question says:

- Longest
- Substring
- Without Repeating Characters

Signals:

- Longest
- Contiguous sequence

Pattern:

- Sliding Window

Template:

- Expand / Shrink Window

Code:

- Python Solution

---

# Universal Question Solving Framework

Whenever you read a problem:

Ask these questions.

## Question 1

Am I searching for something?

Examples:

- Does it exist?
- Have I seen this before?
- Is there a duplicate?

Often indicates:

✅ Hash Map

✅ Hash Set

---

## Question 2

Am I looking at pairs?

Examples:

- Two numbers
- Pair sum
- Pair difference

Often indicates:

✅ Two Pointers

✅ Hash Map

---

## Question 3

Does the problem mention contiguous elements?

Examples:

- Longest substring
- Minimum subarray
- Consecutive characters

Often indicates:

✅ Sliding Window

---

## Question 4

Am I finding the minimum or maximum value that satisfies a condition?

Examples:

- Minimum speed
- Maximum capacity
- Smallest possible answer

Often indicates:

✅ Binary Search on Answer

---

## Question 5

Do I need ALL possibilities?

Examples:

- All permutations
- All combinations
- Every valid arrangement

Often indicates:

✅ Backtracking

---

## Question 6

Do I need the shortest path or minimum steps?

Examples:

- Minimum moves
- Fewest jumps
- Shortest route

Often indicates:

✅ BFS

---

## Question 7

Do I need to explore everything?

Examples:

- Number of islands
- Connected regions
- All paths

Often indicates:

✅ DFS

---

# Pattern 1: Hash Maps & Hash Sets

## Recognition Signals

Look for:

- Duplicate
- Frequency
- Count
- Lookup
- Common elements
- Unique values
- First occurrence
- Seen before

## Immediate Thought

```python
dict
set
Counter
defaultdict
```

## Mental Model

Imagine keeping a notebook.

Every time you see something, you write it down.

When it appears again, you already know.

Instead of searching an entire array every time:

```python
O(n)
```

you instantly check:

```python
O(1)
```

---

## Common LeetCode Questions

- Two Sum
- Contains Duplicate
- Valid Anagram
- Group Anagrams
- Top K Frequent Elements

---

## Trigger Phrase

> "I need fast lookup."

Think Hash Map.

---

# Pattern 2: Two Pointers

## Recognition Signals

Look for:

- Sorted arrays
- Pair problems
- Opposite ends
- Palindromes
- In-place modifications

## Immediate Thought

```python
left = 0
right = n - 1
```

---

## Mental Model

Imagine two people standing at opposite ends of a road.

Each moves based on conditions until they meet.

Instead of checking every pair:

```python
O(n²)
```

You often solve it in:

```python
O(n)
```

---

## Common Problems

- Two Sum II
- Valid Palindrome
- Container With Most Water
- Remove Duplicates from Sorted Array

---

## Trigger Phrase

> "Sorted array + pair problem"

Think Two Pointers.

---

# Pattern 3: Sliding Window

## Recognition Signals

Look for words like:

- Longest
- Shortest
- Maximum
- Minimum
- Subarray
- Substring
- Contiguous
- Consecutive

---

## Immediate Thought

```python
left
right
window
```

---

## Mental Model

Imagine a train window moving across the array.

The window expands.

The window shrinks.

But it never goes backwards.

---

## Common Problems

- Longest Substring Without Repeating Characters
- Minimum Window Substring
- Maximum Average Subarray
- Permutation in String

---

## Trigger Phrase

> "Contiguous sequence"

Think Sliding Window.

---

# Pattern 4: Fast & Slow Pointers

## Recognition Signals

Look for:

- Cycles
- Loops
- Middle node
- Repeated traversal

---

## Immediate Thought

```python
slow += 1
fast += 2
```

---

## Mental Model

Imagine two runners.

One runs twice as fast.

If there is a cycle, they eventually meet.

---

## Common Problems

- Linked List Cycle
- Middle of Linked List
- Happy Number

---

## Trigger Phrase

> "Cycle detection"

Think Fast & Slow Pointer.

---

# Pattern 5: Binary Search

## Recognition Signals

Most people think:

"sorted array"

But that is only half of Binary Search.

More powerful clue:

Look for:

- Minimum possible answer
- Maximum possible answer
- Capacity
- Speed
- Threshold
- Can we achieve X?

---

## Immediate Thought

```python
low
high
mid
```

---

## Mental Model

Imagine looking for a word in a dictionary.

You keep eliminating half the possibilities.

---

## Common Problems

- Binary Search
- Search Rotated Sorted Array
- Koko Eating Bananas
- Capacity To Ship Packages

---

## Trigger Phrase

> "Find the smallest/largest valid answer."

Think Binary Search.

---

# Pattern 6: Stack

## Recognition Signals

Look for:

- Matching brackets
- Undo operations
- Previous greater element
- Previous smaller element
- Nested structures

---

## Immediate Thought

```python
append()
pop()
```

---

## Mental Model

A stack of plates.

Last plate added is the first removed.

LIFO:

Last In First Out

---

## Common Problems

- Valid Parentheses
- Daily Temperatures
- Min Stack
- Next Greater Element

---

## Trigger Phrase

> "Need recent history."

Think Stack.

---

# Pattern 7: Heap

## Recognition Signals

Look for:

- Top K
- Kth Largest
- Kth Smallest
- Highest priority
- Lowest priority

---

## Immediate Thought

```python
heapq
```

---

## Mental Model

A VIP queue.

Most important items stay near the front.

---

## Common Problems

- Kth Largest Element
- Top K Frequent Elements
- Merge K Sorted Lists

---

## Trigger Phrase

> "Top K"

Think Heap.

---

# Pattern 8: DFS

## Recognition Signals

Look for:

- Explore everything
- Connected components
- Islands
- Reachability
- All paths

---

## Immediate Thought

```python
recursion
stack
```

---

## Mental Model

Walk down a path until you cannot continue.

Then come back.

Then try another path.

---

## Common Problems

- Number of Islands
- Max Area of Island
- Same Tree
- Path Sum

---

## Trigger Phrase

> "Explore deeply."

Think DFS.

---

# Pattern 9: BFS

## Recognition Signals

Look for:

- Shortest path
- Minimum distance
- Fewest moves
- Fewest steps
- Levels

---

## Immediate Thought

```python
deque()
```

---

## Mental Model

Drop a stone into water.

The ripple spreads outward.

Everything at distance 1 is visited.

Then distance 2.

Then distance 3.

---

## Common Problems

- Rotting Oranges
- Word Ladder
- Binary Tree Level Order Traversal

---

## Trigger Phrase

> "Minimum steps."

Think BFS.

---

# Pattern 10: Backtracking

## Recognition Signals

Look for:

- Generate all
- All combinations
- All permutations
- All subsets
- Every possible arrangement

---

## Immediate Thought

```python
choose
explore
undo
```

---

## Mental Model

Imagine exploring a maze.

At each junction:

Choose a path.

If it fails:

Go back.

Try another path.

---

## Common Problems

- Subsets
- Permutations
- Combination Sum
- N Queens

---

## Trigger Phrase

> "Need all possibilities."

Think Backtracking.

---

# Pattern 11: Greedy

## Recognition Signals

Look for:

- Maximize
- Minimize
- Scheduling
- Intervals
- Best immediate choice

---

## Mental Model

Take the best decision right now and never revisit it.

Question:

Can a local optimum lead to a global optimum?

If yes:

Greedy may work.

---

## Common Problems

- Jump Game
- Task Scheduler
- Non-overlapping Intervals

---

## Trigger Phrase

> "Best choice right now."

Think Greedy.

---

# Pattern 12: Dynamic Programming

## Recognition Signals

Look for:

- Number of ways
- Maximum profit
- Minimum cost
- Optimization
- Repeated subproblems

---

## Immediate Thought

```python
memoization
tabulation
cache
```

---

## Mental Model

Don't solve the same problem twice.

Store answers.

Reuse them.

---

## Common Problems

- Climbing Stairs
- House Robber
- Coin Change
- Longest Increasing Subsequence

---

## Trigger Phrase

> "I keep solving the same smaller problem."

Think DP.

---

# The Interview Cheat Sheet

If you see...

| Signal | Pattern |
|----------|----------|
| Duplicate / Frequency | Hash Map |
| Sorted Pair Problem | Two Pointers |
| Longest / Shortest Contiguous Segment | Sliding Window |
| Cycle Detection | Fast & Slow Pointer |
| Smallest/Biggest Valid Answer | Binary Search |
| Matching / Previous Element | Stack |
| Top K | Heap |
| Explore Region | DFS |
| Minimum Distance | BFS |
| All Possibilities | Backtracking |
| Local Optimal Choice | Greedy |
| Optimization + Repeated Work | DP |

---

# The Ultimate Goal

A beginner sees:

"Longest Substring Without Repeating Characters"

and thinks:

> "Which algorithm should I use?"

An experienced engineer sees:

- longest
- substring
- contiguous

and immediately thinks:

> Sliding Window

That is the skill we are building.

The goal of DSA is not memorizing hundreds of solutions.

The goal is developing fast pattern recognition so that new problems feel familiar.