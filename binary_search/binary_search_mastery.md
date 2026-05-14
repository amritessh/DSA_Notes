# Binary Search — Complete Revision Guide

> Based on: LeetCode Binary Search for Beginners, LeetCode Explore Card, and curated community patterns.
> Revise this end-to-end before any interview that might involve sorted arrays, optimization, or search.

---

## Table of Contents

1. [Core Intuition](#1-core-intuition)
2. [The Three Templates](#2-the-three-templates)
3. [Overflow-Safe Mid — Never Forget](#3-overflow-safe-mid--never-forget)
4. [The 8 Patterns](#4-the-8-patterns)
5. [Binary Search on Answer — Full Recipe](#5-binary-search-on-answer--full-recipe)
6. [Rotated Array Playbook](#6-rotated-array-playbook)
7. [First & Last Occurrence](#7-first--last-occurrence)
8. [2D Matrix Search](#8-2d-matrix-search)
9. [Classic Bugs & How to Avoid Them](#9-classic-bugs--how-to-avoid-them)
10. [When to Apply Binary Search](#10-when-to-apply-binary-search)
11. [Python bisect / C++ STL](#11-python-bisect--c-stl)
12. [Curated Problem List (50+)](#12-curated-problem-list-50)
13. [4-Week Study Roadmap](#13-4-week-study-roadmap)
14. [Interview Checklist](#14-interview-checklist)

---

## 1. Core Intuition

Binary search works on **any monotonic search space** — not just arrays.

The idea: at every step, you have a window `[lo, hi]` that is guaranteed to contain the answer. You compute a midpoint, check a condition, and **eliminate half the window**. This gives O(log n) time.

```
Array:  [1, 3, 5,  7,  9, 11, 13, 15, 17, 19]
Index:   0  1  2   3   4   5   6   7   8   9
                  mid
Target = 7

Step 1: lo=0, hi=9, mid=4 → nums[4]=9 > 7 → go left, hi=3
Step 2: lo=0, hi=3, mid=1 → nums[1]=3 < 7 → go right, lo=2
Step 3: lo=2, hi=3, mid=2 → nums[2]=5 < 7 → go right, lo=3
Step 4: lo=3, hi=3, mid=3 → nums[3]=7 == 7 → FOUND at index 3
```

**Mental model:** Think of the array as a sequence of `False False False TRUE TRUE TRUE`.
Binary search finds the **boundary** between False and True in O(log n).

---

## 2. The Three Templates

### Template 1 — Exact Match (`while lo <= hi`)

Use when: searching for a specific value in a sorted array.

```python
def binary_search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2      # overflow-safe
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            lo = mid + 1               # target in right half
        else:
            hi = mid - 1               # target in left half
    return -1                          # not found
```

**Key properties:**
- Loop exits when `lo > hi` (empty window)
- `hi` initialised to `n - 1`
- Returns exact index or `-1`

---

### Template 2 — First True / Leftmost Boundary (`while lo < hi`)

Use when: finding the **first index** where a condition becomes True.
Pattern: `F F F F T T T T T` → find the first `T`.

```python
def first_true(nums, target):
    lo, hi = 0, len(nums)              # hi = n (open right boundary)
    while lo < hi:
        mid = (lo + hi) // 2
        if condition(mid):             # e.g. nums[mid] >= target
            hi = mid                   # mid could be answer, shrink right
        else:
            lo = mid + 1              # mid definitely not answer
    return lo                          # first True index (or n if none)
```

**Key properties:**
- Loop exits when `lo == hi`
- `hi` initialised to `n` (one past end) to handle "insert at end"
- Returns `lo` which equals the boundary
- **Most powerful template** — used for lower_bound, first occurrence, BS on answer

---

### Template 3 — Last True / Rightmost Boundary (`while lo < hi`, ceiling mid)

Use when: finding the **last index** where a condition is True.
Pattern: `T T T T T F F F F` → find the last `T`.

```python
def last_true(nums, target):
    lo, hi = -1, len(nums) - 1        # lo = -1 handles "none found"
    while lo < hi:
        mid = (lo + hi + 1) // 2      # CEILING mid — critical to avoid infinite loop
        if condition(mid):             # e.g. nums[mid] <= target
            lo = mid                   # mid could be answer, raise floor
        else:
            hi = mid - 1
    return lo                          # last True index (-1 if none)
```

**Key properties:**
- Uses `(lo + hi + 1) // 2` — ceiling mid — **mandatory** when `lo = mid` to prevent infinite loop
- Returns `lo`

---

### Template Comparison

| | T1 | T2 | T3 |
|---|---|---|---|
| Condition | `lo <= hi` | `lo < hi` | `lo < hi` |
| `hi` init | `n - 1` | `n` | `n - 1` |
| `lo` init | `0` | `0` | `-1` (or 0) |
| Mid formula | `lo + (hi-lo)//2` | `(lo+hi)//2` | `(lo+hi+1)//2` |
| When lo moves | `lo = mid + 1` | `lo = mid + 1` | `lo = mid` |
| When hi moves | `hi = mid - 1` | `hi = mid` | `hi = mid - 1` |
| Returns | index or -1 | `lo` (first True) | `lo` (last True) |
| Use case | exact match | first occurrence, lower bound | last occurrence, upper bound |

---

## 3. Overflow-Safe Mid — Never Forget

```python
# WRONG — overflows in C++/Java when lo + hi > INT_MAX
mid = (lo + hi) // 2

# CORRECT — always use this
mid = lo + (hi - lo) // 2

# Python has arbitrary-precision ints, so both work,
# but use the safe form as a habit for coding interviews.

# Ceiling mid (Template 3):
mid = lo + (hi - lo + 1) // 2
# or equivalently:
mid = (lo + hi + 1) // 2
```

---

## 4. The 8 Patterns

### Pattern 1 — Classic Exact Search

**Trigger:** Sorted array, find a specific value.
**Template:** T1

```python
# LC 704 - Binary Search
def search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] == target: return mid
        elif nums[mid] < target: lo = mid + 1
        else: hi = mid - 1
    return -1
```

**Problems:** #704, #35, #374, #69, #367

---

### Pattern 2 — First / Last Occurrence

**Trigger:** Array with duplicates, find leftmost or rightmost index.
**Template:** T2 for first, T3 for last.

```python
# First occurrence (Template 2)
def first_occurrence(nums, target):
    lo, hi = 0, len(nums)
    while lo < hi:
        mid = (lo + hi) // 2
        if nums[mid] >= target:
            hi = mid
        else:
            lo = mid + 1
    return lo if lo < len(nums) and nums[lo] == target else -1

# Last occurrence (Template 3)
def last_occurrence(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo < hi:
        mid = (lo + hi + 1) // 2
        if nums[mid] <= target:
            lo = mid
        else:
            hi = mid - 1
    return lo if nums[lo] == target else -1
```

**Problems:** #34, #744, #278, #275, #1385

---

### Pattern 3 — Rotated Sorted Array

**Trigger:** Array was sorted then rotated. Find element or minimum.
**Template:** T1 with extra half-sorted check.

**Key insight:** In a rotated array, when you split at mid, **one half is always sorted**.
Check which half is sorted, then determine if the target lies in it.

```python
# LC 33 - Search in Rotated Sorted Array
def search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] == target:
            return mid
        # Left half is sorted
        if nums[lo] <= nums[mid]:
            if nums[lo] <= target < nums[mid]:
                hi = mid - 1          # target in sorted left half
            else:
                lo = mid + 1          # target in right half
        # Right half is sorted
        else:
            if nums[mid] < target <= nums[hi]:
                lo = mid + 1          # target in sorted right half
            else:
                hi = mid - 1          # target in left half
    return -1

# LC 153 - Find Minimum in Rotated Sorted Array
def findMin(nums):
    lo, hi = 0, len(nums) - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if nums[mid] > nums[hi]:
            lo = mid + 1              # min is in right half
        else:
            hi = mid                  # mid could be min
    return nums[lo]
```

**Problems:** #33, #81, #153, #154, #162

---

### Pattern 4 — Binary Search on Answer (Most Powerful)

**Trigger:** "Find minimum X such that..." or "Find maximum X such that..."
Keywords: *minimum speed, maximum capacity, minimum days, at least k, split into m parts*
**Template:** T2 (first True) or T3 (last True) on the answer space.

**Full recipe — see Section 5 below.**

**Problems:** #875, #1011, #1482, #410, #1283, #1552, #2064

---

### Pattern 5 — 2D Matrix Search

**Trigger:** Sorted matrix, find a value.
**Template:** T1 with index mapping.

```python
# LC 74 - Search a 2D Matrix (rows and columns sorted, rows chain)
def searchMatrix(matrix, target):
    m, n = len(matrix), len(matrix[0])
    lo, hi = 0, m * n - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        val = matrix[mid // n][mid % n]   # KEY: 1D to 2D mapping
        if val == target: return True
        elif val < target: lo = mid + 1
        else: hi = mid - 1
    return False

# LC 240 - Search a 2D Matrix II (each row/col sorted independently)
# Use staircase search: start top-right, O(m+n)
def searchMatrix(matrix, target):
    r, c = 0, len(matrix[0]) - 1
    while r < len(matrix) and c >= 0:
        if matrix[r][c] == target: return True
        elif matrix[r][c] > target: c -= 1
        else: r += 1
    return False
```

**Problems:** #74, #240, #378

---

### Pattern 6 — BS + Greedy Feasibility Check

**Trigger:** Binary search on answer, feasibility check requires scanning all n elements.
**Complexity:** O(n log(answer_range))

```python
# Template: binary search on answer, greedy O(n) check
def solve():
    lo, hi = min_possible, max_possible
    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid                   # try smaller
        else:
            lo = mid + 1
    return lo

def feasible(capacity):
    # O(n) greedy simulation
    days, current = 1, 0
    for w in weights:
        if current + w > capacity:
            days += 1
            current = 0
        current += w
    return days <= D
```

**Problems:** #1231, #774, #1760, #2226, #2517

---

### Pattern 7 — Floating-Point Binary Search

**Trigger:** Answer is a real number (not integer). Loop on `eps` instead of integer convergence.

```python
def solve():
    lo, hi = 0.0, max_possible
    for _ in range(100):              # fixed iterations, ~1e-30 precision
        mid = (lo + hi) / 2.0
        if feasible(mid):
            hi = mid
        else:
            lo = mid
    return lo
```

Or: `while hi - lo > 1e-7: ...`

**Problems:** #644, #1870, #786

---

### Pattern 8 — Hard / Multi-Condition

**Trigger:** Two simultaneous binary searches, tricky invariants, or BS as a subroutine.

```python
# LC 4 - Median of Two Sorted Arrays — BS on partition of smaller array
def findMedianSortedArrays(nums1, nums2):
    if len(nums1) > len(nums2):
        return findMedianSortedArrays(nums2, nums1)
    m, n = len(nums1), len(nums2)
    lo, hi = 0, m
    while lo <= hi:
        i = (lo + hi) // 2
        j = (m + n + 1) // 2 - i
        maxL1 = float('-inf') if i == 0 else nums1[i-1]
        minR1 = float('inf')  if i == m else nums1[i]
        maxL2 = float('-inf') if j == 0 else nums2[j-1]
        minR2 = float('inf')  if j == n else nums2[j]
        if maxL1 <= minR2 and maxL2 <= minR1:
            if (m + n) % 2 == 1:
                return max(maxL1, maxL2)
            return (max(maxL1, maxL2) + min(minR1, minR2)) / 2
        elif maxL1 > minR2:
            hi = i - 1
        else:
            lo = i + 1
```

**Problems:** #4, #1095, #327, #302, #668

---

## 5. Binary Search on Answer — Full Recipe

This pattern solves problems of the form:
*"Find the minimum X such that condition(X) is achievable"*
*"Find the maximum X such that condition(X) is achievable"*

### Step-by-step

**Step 1: Identify the search space.**
What is the minimum possible answer? What is the maximum?
These become `lo` and `hi`.

**Step 2: Write the predicate `feasible(mid)`.**
This function returns `True` if `mid` is achievable, `False` otherwise.
It usually involves a greedy simulation over the input.

**Step 3: Verify monotonicity.**
If `feasible(x)` is True, is `feasible(x+1)` also True?
The predicate must be monotonic: `F F F F T T T T T` (or `T T T T F F F F`).

**Step 4: Apply the right template.**
- Find minimum X where feasible is True → Template 2 (first True)
- Find maximum X where feasible is True → Template 3 (last True)

**Step 5: Check boundary edge cases.**
After the loop, `lo == hi`. Verify the result is valid.

### Example — Koko Eating Bananas (#875)

```python
def minEatingSpeed(piles, h):
    # Step 1: search space is [1, max(piles)]
    lo, hi = 1, max(piles)

    # Step 2 + 3: feasible — can Koko finish at speed k?
    def feasible(k):
        return sum(math.ceil(p / k) for p in piles) <= h
    # Monotonic: if feasible(k) then feasible(k+1) (faster = fewer hours)

    # Step 4: find minimum k where feasible is True
    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid           # could be answer, try smaller
        else:
            lo = mid + 1      # too slow, need faster

    return lo                  # minimum speed
```

### Example — Capacity to Ship Packages (#1011)

```python
def shipWithinDays(weights, days):
    lo = max(weights)          # must carry heaviest package
    hi = sum(weights)          # carry everything in one day

    def feasible(cap):
        d, cur = 1, 0
        for w in weights:
            if cur + w > cap:
                d += 1; cur = 0
            cur += w
        return d <= days

    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid): hi = mid
        else: lo = mid + 1
    return lo
```

### Common answer space bounds

| Problem type | `lo` | `hi` |
|---|---|---|
| Minimum eating speed | `1` | `max(piles)` |
| Ship in D days | `max(weights)` | `sum(weights)` |
| Split into k parts | `max(arr)` | `sum(arr)` |
| Minimum days for m bouquets | `1` | `max(bloomDay)` |
| Smallest divisor | `1` | `max(arr)` |
| Kth smallest in matrix | `matrix[0][0]` | `matrix[-1][-1]` |

---

## 6. Rotated Array Playbook

```
Original: [1, 2, 3, 4, 5, 6, 7]
Rotated:  [4, 5, 6, 7, 1, 2, 3]   rotation point at index 4
```

**Rule:** When you split at `mid`, one half is **always** fully sorted.
Check `nums[lo] <= nums[mid]`:
- True → left half `[lo..mid]` is sorted
- False → right half `[mid..hi]` is sorted

Then check if target falls in the sorted half. If yes, search there; if no, search the other half.

**Finding the minimum (rotation point):**

```python
# nums[mid] > nums[hi] means the minimum is in the right half
if nums[mid] > nums[hi]:
    lo = mid + 1
else:
    hi = mid
```

**With duplicates (#81, #154):** When `nums[lo] == nums[mid] == nums[hi]`, you can't determine which half is sorted. Shrink both boundaries: `lo += 1; hi -= 1`.

---

## 7. First & Last Occurrence

The most common beginner mistake: returning immediately when you find the target.
Instead, **save the answer and keep searching** in the appropriate direction.

```python
def searchRange(nums, target):
    def find_first():
        lo, hi, ans = 0, len(nums) - 1, -1
        while lo <= hi:
            mid = (lo + hi) // 2
            if nums[mid] == target:
                ans = mid
                hi = mid - 1       # keep searching LEFT for earlier occurrence
            elif nums[mid] < target:
                lo = mid + 1
            else:
                hi = mid - 1
        return ans

    def find_last():
        lo, hi, ans = 0, len(nums) - 1, -1
        while lo <= hi:
            mid = (lo + hi) // 2
            if nums[mid] == target:
                ans = mid
                lo = mid + 1       # keep searching RIGHT for later occurrence
            elif nums[mid] < target:
                lo = mid + 1
            else:
                hi = mid - 1
        return ans

    return [find_first(), find_last()]
```

**Alternative using Template 2 / lower_upper_bound:**

```python
# First occurrence = lower_bound = first index where nums[i] >= target
lo, hi = 0, len(nums)
while lo < hi:
    mid = (lo + hi) // 2
    if nums[mid] >= target: hi = mid
    else: lo = mid + 1
first = lo if lo < len(nums) and nums[lo] == target else -1

# Last occurrence = upper_bound - 1 = last index where nums[i] <= target
lo, hi = 0, len(nums)
while lo < hi:
    mid = (lo + hi) // 2
    if nums[mid] > target: hi = mid
    else: lo = mid + 1
last = lo - 1 if lo > 0 and nums[lo-1] == target else -1
```

---

## 8. 2D Matrix Search

**LC 74 (rows chain — true sorted 1D):** Map 1D index to 2D.

```python
row = mid // n
col = mid % n
```

**LC 240 (each row/col sorted independently):** Use staircase search from top-right.

```python
r, c = 0, n - 1
while r < m and c >= 0:
    if matrix[r][c] == target: return True
    elif matrix[r][c] > target: c -= 1
    else: r += 1
```

**LC 378 (kth smallest in sorted matrix):** Binary search on value range, count elements <= mid.

```python
lo, hi = matrix[0][0], matrix[-1][-1]
while lo < hi:
    mid = (lo + hi) // 2
    count = sum(bisect_right(row, mid) for row in matrix)
    if count >= k: hi = mid
    else: lo = mid + 1
return lo
```

---

## 9. Classic Bugs & How to Avoid Them

### Bug 1 — Integer overflow

```cpp
// WRONG (C++/Java): lo=2e9, hi=2e9 overflows int
int mid = (lo + hi) / 2;

// CORRECT:
int mid = lo + (hi - lo) / 2;
```

### Bug 2 — Wrong `hi` initialisation

```python
# Searching for insert position: answer could be n (after all elements)
hi = len(nums)     # NOT len(nums) - 1
```

### Bug 3 — Infinite loop with `lo = mid` (Template 3)

```python
# If lo = hi - 1, mid = (lo+hi)//2 = lo, then lo = mid = lo → infinite!
# FIX: use ceiling mid when lo = mid
mid = (lo + hi + 1) // 2
```

### Bug 4 — Not checking result after loop

```python
while lo < hi:
    ...
# After loop, lo == hi. Verify before returning:
return lo if nums[lo] == target else -1
```

### Bug 5 — Wrong condition direction

```python
# condition is "nums[mid] >= target" → Template 2 (move hi=mid when True)
# condition is "nums[mid] <= target" → Template 3 (move lo=mid when True)
# Getting this backwards is the most common logic error.
```

### Bug 6 — Off-by-one on boundary check

```python
# After finding first_true with T2, result is in range [0, n]
# n means condition was never True — handle this!
lo = first_true(...)
if lo == len(nums):
    return -1   # no valid answer
```

---

## 10. When to Apply Binary Search

### Direct signals

- Array is **sorted** (or sortable)
- Problem asks for **minimum X** such that some condition holds
- Problem asks for **maximum X** such that some condition holds
- Brute-force is O(n²), hint toward O(n log n)

### Monotonicity test

Write your proposed predicate for small inputs. Does it look like:

```
F F F F T T T T T  → Template 2, find first T
T T T T T F F F F  → Template 3, find last T
```

If yes, binary search applies.

### Common answer spaces

| Problem type | Search space |
|---|---|
| Minimum eating speed | `[1, max(piles)]` |
| Ship in D days | `[max(weights), sum(weights)]` |
| Split into k parts | `[max(arr), sum(arr)]` |
| Minimum days for m bouquets | `[1, max(bloomDay)]` |
| Smallest divisor | `[1, max(arr)]` |
| Kth smallest | `[min_val, max_val]` |

### Red flags (BS probably doesn't apply)

- Unsorted array with no monotonic property
- Answer depends on non-monotonic function
- Need all solutions, not just the boundary

---

## 11. Python `bisect` / C++ STL

### Python bisect

```python
import bisect

a = [1, 3, 5, 7, 9]

# First index where a[i] >= x  (lower_bound)
bisect.bisect_left(a, 5)    # 2

# First index where a[i] > x  (upper_bound)
bisect.bisect_right(a, 5)   # 3

# Number of elements == x
bisect.bisect_right(a, x) - bisect.bisect_left(a, x)

# Insert x maintaining sorted order
bisect.insort_left(a, x)    # insert before equal elements
bisect.insort_right(a, x)   # insert after equal elements
```

### C++ STL

```cpp
vector<int> v = {1, 3, 5, 7, 9};

// First position >= target (lower_bound)
auto it = lower_bound(v.begin(), v.end(), 5);  // index 2

// First position > target (upper_bound)
auto it = upper_bound(v.begin(), v.end(), 5);  // index 3

// Check existence
bool found = binary_search(v.begin(), v.end(), 5);  // true

// Distance from begin (= index)
int idx = lower_bound(v.begin(), v.end(), 5) - v.begin();
```

---

## 12. Curated Problem List (50+)

### Easy — do these first

| # | Problem | Pattern | Key idea |
|---|---|---|---|
| 704 | Binary Search | Classic | T1 baseline |
| 35 | Search Insert Position | Classic | T2, hi=n |
| 278 | First Bad Version | First/Last | T2 on abstract array |
| 374 | Guess Number Higher or Lower | Classic | T1, API calls |
| 69 | Sqrt(x) | Classic | T1, ans = hi when done |
| 367 | Valid Perfect Square | Classic | T1, mid*mid == num |
| 744 | Find Smallest Letter Greater Than Target | First/Last | T2, circular |
| 1385 | Find the Distance Value Between Two Arrays | Classic | BS per element |

### Medium — core patterns

| # | Problem | Pattern | Key idea |
|---|---|---|---|
| 34 | Find First and Last Position of Element | First/Last | T2 + T3 both |
| 33 | Search in Rotated Sorted Array | Rotated | Half-sorted check |
| 153 | Find Minimum in Rotated Sorted Array | Rotated | Compare mid to hi |
| 162 | Find Peak Element | Rotated | nums[mid] vs nums[mid+1] |
| 74 | Search a 2D Matrix | 2D | 1D index mapping |
| 240 | Search a 2D Matrix II | 2D | Staircase search |
| 275 | H-Index II | First/Last | T2, find first i where condition |
| 875 | Koko Eating Bananas | BS on Answer | feasible = finish in h hours |
| 1011 | Capacity to Ship Packages Within D Days | BS on Answer | feasible = ship in D days |
| 1283 | Find the Smallest Divisor Given a Threshold | BS on Answer | feasible = sum of ceilings <= threshold |
| 1482 | Minimum Number of Days to Make M Bouquets | BS on Answer | feasible = make m bouquets |
| 1552 | Magnetic Force Between Two Balls | BS on Answer | feasible = place m balls with >= force |
| 378 | Kth Smallest Element in a Sorted Matrix | 2D | BS on value + count |
| 786 | Kth Smallest Prime Fraction | Advanced | BS on value |
| 1870 | Minimum Speed to Arrive on Time | BS on Answer | floating point feasibility |
| 2226 | Maximum Candies Allocated to K Children | BS on Answer | maximize candies per child |
| 1760 | Minimum Limit of Balls in a Bag | BS on Answer | feasible = ops needed <= maxOps |
| 2064 | Minimized Maximum of Products Distributed | BS on Answer | feasible = max load <= mid |

### Hard — master level

| # | Problem | Pattern | Key idea |
|---|---|---|---|
| 4 | Median of Two Sorted Arrays | Multi-BS | Partition both arrays |
| 410 | Split Array Largest Sum | BS on Answer | Minimize the maximum subarray sum |
| 154 | Find Minimum in Rotated Sorted Array II | Rotated | Handle duplicates: shrink both ends |
| 81 | Search in Rotated Sorted Array II | Rotated | Handle duplicates: lo++, hi-- |
| 1095 | Find in Mountain Array | Advanced | Find peak, BS left, BS right |
| 644 | Maximum Average Subarray II | Float BS | BS on answer + prefix sum check |
| 327 | Count of Range Sum | Advanced | BS in merge sort |
| 302 | Smallest Rectangle Enclosing Black Pixels | Advanced | BS on row/col boundaries |
| 668 | Kth Smallest Number in Multiplication Table | Advanced | Count elements <= mid |
| 774 | Minimize Max Distance to Gas Station | BS + Greedy | Float BS + greedy insertion |
| 1231 | Divide Chocolate | BS + Greedy | Maximize minimum sweetness |

---

## 13. 4-Week Study Roadmap

### Week 1 — Foundation

**Goal:** Write T1 from memory with zero bugs.

- Study Template 1
- Solve: #704, #35, #374, #69, #278, #367
- Practice: dry-run arrays of size 1, 2, 3 to catch edge cases
- Time target: Easy ≤ 10 minutes

### Week 2 — Boundary Patterns

**Goal:** Identify which template to use in under 60 seconds.

- Study Templates 2 and 3
- Solve: #34, #744, #275, #74, #153, #33
- Practice: for each problem, write the T/F array before coding
- Time target: Medium ≤ 20 minutes

### Week 3 — Binary Search on Answer

**Goal:** Write `feasible()` predicates confidently for any optimization problem.

- Study Pattern 4 and Section 5 (full recipe)
- Solve: #875, #1011, #1482, #1283, #410, #1760, #2226
- Practice: for each problem, explicitly identify lo, hi, and the monotonicity argument
- Time target: Medium ≤ 20 minutes

### Week 4 — Advanced & Mixed

**Goal:** Handle multi-condition and tricky invariant problems.

- Study Patterns 6, 7, 8
- Solve: #4, #1095, #162, #378, #668, #786
- Practice: explain your invariant out loud before touching the keyboard
- Time target: Hard ≤ 35 minutes

---

## 14. Interview Checklist

Use this before writing any binary search solution.

```
Before coding:
  Is the search space sorted / monotonic?
  What is lo? What is hi?
  State the invariant: "the answer is always in [lo, hi]"
  Which template? T1 (exact), T2 (first True), T3 (last True)
  If BS on answer: what is feasible(mid)? Is it monotonic?
  Floor mid or ceiling mid? (T3 needs ceiling to avoid infinite loop)
  What does the loop return? Is post-loop validation needed?
  Edge cases: empty array, single element, all same, not found

In code:
  mid = lo + (hi - lo) // 2         not (lo+hi)//2
  hi initialised to n (not n-1) if answer could be at end
  Correct condition direction (>= or > ?)
  Correct pointer movement (hi=mid vs hi=mid-1)
  After loop: lo == hi — verify validity before returning
```

---

## Quick Reference Card

```
EXACT MATCH                FIRST TRUE                 LAST TRUE
────────────────           ────────────────           ────────────────
lo, hi = 0, n-1            lo, hi = 0, n              lo, hi = -1, n-1
while lo <= hi:             while lo < hi:              while lo < hi:
  mid=lo+(hi-lo)//2          mid=(lo+hi)//2               mid=(lo+hi+1)//2
  if hit: return mid          if cond: hi=mid              if cond: lo=mid
  if <: lo=mid+1             else: lo=mid+1               else: hi=mid-1
  else: hi=mid-1            return lo                    return lo
return -1

BS ON ANSWER
────────────────────────────────────────────
1. lo = min_possible, hi = max_possible
2. def feasible(mid): ...   O(n) greedy check
3. Apply FIRST TRUE (minimize) or LAST TRUE (maximize)
4. return lo
```

---

*Sources: LeetCode Binary Search Explore Card · "Binary Search for Beginners" (LC discuss, June 2020) · "Binary Search: A Comprehensive Guide" (LC discuss) · "Binary Search on Answer" by Amit Maity (Medium, 2023)*
