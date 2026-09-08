# Two Pointers Pattern Recognition Cheat Sheet

The purpose of this file is not to memorize solutions.

The purpose is to train pattern recognition.

For every problem, identify:

```text
Signal
↓
Question
↓
Pointer Type
↓
Pattern
↓
Representative Problem
```

---

# Pattern 1: Opposite Direction Pointers

## Visual

```text
left                     right
 |                           |

[1,2,3,4,5,6,7,8]

    ->               <-
```

---

## Signals

Look for:

```text
Sorted Array
Pair Sum
Palindrome
Compare Ends
Max Area
```

---

## Question

Ask:

```text
Can information from the current comparison tell me
which side can be safely discarded?
```

---

## Pointer Type

```python
left = 0
right = len(array) - 1
```

---

## Movement

```text
Move Left
Move Right
Move Both
```

depending on the comparison result.

---

## Representative Problems

```text
LC 125 - Valid Palindrome
LC 167 - Two Sum II
LC 11  - Container With Most Water
LC 15  - 3Sum
```

---

# Pattern 2: Read / Write Pointers

## Visual

```text
write
 |

[0,1,0,3,12]

      |
     read
```

---

## Signals

Look for:

```text
Move Elements
Remove Elements
Remove Duplicates
Modify In Place
Compact Array
```

---

## Question

Ask:

```text
Where should the next valid value be written?
```

---

## Pointer Type

```python
read
write
```

---

## Movement

```text
Read scans everything.
Write only moves when valid data is found.
```

---

## Representative Problems

```text
LC 283 - Move Zeroes
LC 26  - Remove Duplicates
LC 27  - Remove Element
```

---

# Pattern 3: Fast and Slow Pointers

## Visual

```text
slow -> 1 step

fast -> 2 steps
```

---

## Signals

Look for:

```text
Cycle
Linked List
Middle Node
Repeated Path
```

---

## Question

Ask:

```text
Can different speeds reveal a cycle or midpoint?
```

---

## Pointer Type

```python
slow
fast
```

---

## Representative Problems

```text
LC 141 - Linked List Cycle
LC 876 - Middle of Linked List
```

---

# Pattern 4: Fixed Value + Two Moving Pointers

This is an extension of Opposite-Direction Pointers.

---

## Visual

Fix one value.

```text
i

[-4,-1,-1,0,1,2]

      L       R
```

---

## Signals

Look for:

```text
Triplet
3 Sum
Combination
Three Numbers
```

---

## Question

Ask:

```text
Can I fix one value
and solve the remaining problem with two pointers?
```

---

## Representative Problems

```text
LC 15 - 3Sum
LC 18 - 4Sum
```

---

# Recognition Decision Tree

## Sorted Array?

```text
YES
↓
Can I use Left + Right?
↓
Two Pointers
```

---

## Palindrome?

```text
YES
↓
Compare Ends
↓
Two Pointers
```

---

## Move Elements?

```text
YES
↓
Read / Write
↓
Two Pointers
```

---

## Cycle?

```text
YES
↓
Fast / Slow
↓
Two Pointers
```

---

## Triplet?

```text
YES
↓
Fix One Value
+
Two Pointers
```

---

# Hashing vs Two Pointers

## Hashing Asks

```text
What have I seen before?
```

Examples:

```text
Contains Duplicate
Two Sum
Happy Number
```

---

## Two Pointers Asks

```text
Which pointer should move?
```

Examples:

```text
Palindrome
Two Sum II
Container With Most Water
3Sum
```

---

# Signals To Memorize

| Signal | Think |
|----------|----------|
| Sorted Array | Two Pointers |
| Palindrome | Two Pointers |
| Compare Ends | Two Pointers |
| Pair Sum in Sorted Array | Two Pointers |
| Move Zeroes | Read/Write |
| Remove Duplicates | Read/Write |
| Cycle | Fast/Slow |
| Triplet Sum | Fix One + Two Pointers |

---

# Biggest Two Pointer Mistake

Do NOT memorize:

```python
left += 1
right -= 1
```

Instead ask:

```text
Why is moving this pointer safe?
```

That reasoning is the actual pattern.

---

# What To Memorize

```text
Sorted Pair
↓
Opposite Direction

Move Elements
↓
Read / Write

Cycle
↓
Fast / Slow

Triplets
↓
Fix One + Two Pointers

Core Question:
Which pointer should move and why?
```
