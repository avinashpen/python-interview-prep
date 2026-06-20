# PART I — Data Structures & Collections

## 1. Master Complexity Table

| Structure | Access | Search | Insert | Delete | Notes |
|---|---|---|---|---|---|
| `list` | O(1) | O(n) | append O(1)*, insert(i) O(n) | pop() O(1), pop(0) O(n) | contiguous array |
| `dict` | O(1) avg | O(1) avg | O(1) avg | O(1) avg | insertion-ordered (3.7+) |
| `set` / `frozenset` | — | O(1) avg | O(1) avg | O(1) avg | hash-based, no order |
| `deque` | O(n) | O(n) | append/appendleft O(1) | pop/popleft O(1) | O(1) both ends |
| `heapq` (list) | heap[0] O(1) | O(n) | heappush O(log n) | heappop O(log n) | heapify O(n) |
| `SortedList` | O(log n) by index | O(log n) | O(log n) | O(log n) | third-party; ordered |
| `str` | O(1) | O(n) | immutable | immutable | build via list + join |

**\* amortized.** The two facts interviewers probe: `list.pop(0)` / `list.insert(0, x)` are O(n) — use a deque; and `x in list` is O(n) while `x in set` is O(1).

## 2. list — methods & complexity

| Method | Complexity | Note |
|---|---|---|
| `append(x)` | O(1) amortized | stack push |
| `pop()` | O(1) | stack pop |
| `pop(i)` | O(n) | shifts tail |
| `insert(i, x)` | O(n) | shifts tail |
| `remove(x)` | O(n) | first match |
| `index(x)` | O(n) | raises if absent |
| `count(x)` | O(n) | |
| `x in arr` | O(n) | linear scan |
| `sort(key=, reverse=)` | O(n log n) | in place, stable (Timsort) |
| `reverse()` | O(n) | in place |
| `extend(it)` | O(k) | |
| `arr[i:j]` | O(j−i) | copies |

```python
# 2D fill — NOT [[0]*cols]*rows, which aliases every row
grid = [[0] * cols for _ in range(rows)]

# multi-key sort, mixed direction
arr.sort(key=lambda p: (p[0], -p[1]))

# reversed copy
rev = arr[::-1]
```

## 3. dict — methods & complexity

| Method | Complexity | Note |
|---|---|---|
| `d[k]` / `d[k]=v` / `del d[k]` | O(1) avg | |
| `d.get(k, default)` | O(1) avg | no KeyError |
| `d.setdefault(k, default)` | O(1) avg | group without defaultdict |
| `d.pop(k, default)` | O(1) avg | |
| `d.popitem()` | O(1) | LIFO (3.7+) |
| `k in d` | O(1) avg | |
| `d.update(other)` | O(len other) | |
| `d.keys/values/items()` | O(1) view | lazy views |
| `d1 \| d2` (3.9+) | O(len both) | merge; `\|=` to mutate |

## 4. set / frozenset — methods & complexity

| Method | Complexity | Note |
|---|---|---|
| `add(x)` | O(1) avg | |
| `remove(x)` | O(1) avg | KeyError if absent |
| `discard(x)` | O(1) avg | silent if absent |
| `pop()` | O(1) | arbitrary element |
| `x in s` | O(1) avg | |
| `a \| b` (union) | O(len a + len b) | |
| `a & b` (intersection) | O(min(len a, len b)) | |
| `a - b` (difference) | O(len a) | |
| `a ^ b` (sym. diff) | O(len a + len b) | |
| `a <= b` (issubset) | O(len a) | |

`frozenset` is hashable — use it as a dict key or set element (e.g. canonical visited states).

## 5. str — key facts

Immutable. Building with `s += c` in a loop is O(n²); collect into a list and `"".join(parts)` for O(n). `ord('a')` / `chr(97)` for char↔int bucket counting. `s.split()`, `s.rsplit(maxsplit=1)`.

## 6. collections.deque — methods & complexity

| Method | Complexity | Note |
|---|---|---|
| `append` / `appendleft` | O(1) | |
| `pop` / `popleft` | O(1) | BFS uses popleft |
| `extend` / `extendleft` | O(k) | |
| `rotate(n)` | O(min(n, len)) | |
| `dq[i]` | O(n) | O(1) only at ends |
| `remove(x)` / `index(x)` | O(n) | |
| `deque(maxlen=k)` | — | auto-evicts oldest |

Use for: BFS, sliding-window, monotonic queue.

## 7. collections.defaultdict

Same complexity as dict; supplies a default via a factory on missing-key access. `defaultdict(list)` for adjacency lists, `defaultdict(int)` for counting, `defaultdict(set)` for grouping. Gotcha: read access *creates* the key — use plain `dict.get` if you must not mutate.

## 8. collections.Counter — methods & complexity

| Method | Complexity | Note |
|---|---|---|
| `Counter(iterable)` | O(n) | frequency map |
| `most_common(k)` | O(n log k) | O(n log n) if k=None |
| `elements()` | O(total count) | |
| `update` / `subtract` | O(n) | |
| `c1 + c2`, `c1 - c2`, `c1 & c2`, `c1 \| c2` | O(n) | multiset arithmetic |
| `total()` (3.10+) | O(n) | sum of counts |

`Counter(s1) == Counter(s2)` is a clean anagram check.

## 9. collections.OrderedDict — LRU backbone

| Method | Complexity | Note |
|---|---|---|
| `move_to_end(k, last=True)` | O(1) | mark most-recently-used |
| `popitem(last=False)` | O(1) | evict least-recently-used |
| all dict ops | same as dict | |

## 10. collections.namedtuple

Immutable lightweight record; field access by name. Readable heap entries: `Item = namedtuple("Item", "priority node")`. Zero method-complexity overhead vs a plain tuple.

## 11. heapq — functions & complexity

| Function | Complexity | Note |
|---|---|---|
| `heapify(arr)` | O(n) | in place, min-heap |
| `heappush(h, x)` | O(log n) | |
| `heappop(h)` | O(log n) | |
| `h[0]` | O(1) | peek min |
| `heappushpop(h, x)` | O(log n) | push then pop, one sift |
| `heapreplace(h, x)` | O(log n) | pop then push |
| `nlargest/nsmallest(k, it)` | O(n log k) | |
| `merge(*iters)` | O(n log k) | merge k sorted |

```python
# max-heap: negate values on the way in and out
heapq.heappush(h, -x)

# tie-break with a counter so heap never compares the nodes themselves
heapq.heappush(h, (dist, next(counter), node))
```

## 12. bisect — functions & complexity

| Function | Complexity | Note |
|---|---|---|
| `bisect_left(a, x)` | O(log n) | first index >= x |
| `bisect_right(a, x)` | O(log n) | first index > x |
| `insort_left/right(a, x)` | O(n) | log n search + O(n) shift |

Count of x in sorted `a`: `bisect_right(a, x) - bisect_left(a, x)`. The O(n) insort is exactly why repeated insertion into a sorted list is O(n²) — switch to SortedList below.

## 13. THIRD-PARTY: sortedcontainers (deep dive)

`pip install sortedcontainers` — `from sortedcontainers import SortedList, SortedDict, SortedSet`

### How it actually works (the part that confuses people)

It is **NOT a balanced binary search tree.** Internally it is a **list of sorted sublists** (plus a "maxes" index of each sublist's largest element, and a positional index). To add a value: bisect the maxes to pick the target sublist (O(log(n/load))), then `list.insert` into that sublist. Sublists split/merge to stay near a load factor (default 1000).

Consequence: the per-op sublist insert is technically O(√n) with a tiny constant, but bisect plus contiguous-memory C-level list ops make it **faster than tree structures in CPython.** For interviews, analyze add/remove/index as **O(log n)**, but say out loud that it's a list-of-lists, not a tree — that candor signals you actually know the library.

### SortedList — methods & complexity

| Method | Complexity | Note |
|---|---|---|
| `add(x)` | O(log n) | keeps sorted |
| `remove(x)` / `discard(x)` | O(log n) | **arbitrary** element removal |
| `pop(i=-1)` | O(log n) | |
| `sl[i]` | O(log n) | indexed access — order statistics |
| `bisect_left/right(x)` | O(log n) | |
| `index(x)` / `count(x)` | O(log n) | |
| `x in sl` | O(log n) | |
| `irange(lo, hi)` | O(log n + k) | range iteration |
| `update(it)` | O(k log n) | |

### SortedDict — methods & complexity (sorted by KEY)

Wraps a regular dict (values) + a SortedList of keys. Value lookup is dict-fast, but key insertion/deletion pays the sorted cost.

| Method | Complexity | Note |
|---|---|---|
| `sd[k]` (read) | O(1) | dict hit |
| `sd[k] = v` (new key) | O(log n) | insert into key list |
| `sd[k] = v` (existing) | O(1) | value overwrite |
| `del sd[k]` | O(log n) | |
| `k in sd` | O(1) | |
| `peekitem(i)` | O(log n) | i-th smallest key |
| `bisect_left/right(k)` | O(log n) | on KEYS, not values |
| `irange(lo, hi)` | O(log n + k) | |

### SortedSet — methods & complexity

Wraps a regular set (membership) + a SortedList (order). Membership is O(1), but mutation pays the sorted cost.

| Method | Complexity | Note |
|---|---|---|
| `x in ss` | O(1) | backed by set |
| `add(x)` | O(log n) | SortedList insert |
| `discard(x)` / `remove(x)` | O(log n) | |
| `ss[i]` | O(log n) | indexed |
| `bisect_left/right(x)` | O(log n) | |
| union / intersection / difference | O(n log n) | |

### Which one: the decision that trips people up

| Tool | Gives you | Cannot do | Use when |
|---|---|---|---|
| `heapq` | O(1) peek min, O(log n) push/pop | no arbitrary remove, no ordered iteration, no indexing | top-K, streaming min/max, Dijkstra, merge-K |
| `bisect` + list | O(log n) search | O(n) insert/delete (array shift) | set is static/small, or search-only |
| `SortedList` | O(log n) add/remove of ANY element + index + ordered iteration | slightly slower constants than heapq for pure top-K | dynamic ordered set: sliding-window median, kth-largest with removals, count-in-range over a changing set |

### Common mistakes

- Assuming heapq can remove or find an arbitrary element — it can't; you'd need lazy deletion (push `(value, version)`, skip stale entries on pop).
- Calling `bisect.insort` in a loop → O(n²). Use SortedList for O(n log n).
- Mutating an element already inside a SortedList breaks ordering — remove, change, re-add.
- SortedDict `bisect` works on **keys**, not values.
- **Availability:** present on LeetCode; **NOT guaranteed** on CodeSignal / HackerRank / Codility. For a CodeSignal OA, prepare a heapq + lazy-deletion fallback, or hand-roll, before depending on SortedList.

## 14. itertools / functools quick hits

```python
from itertools import accumulate, chain, combinations, permutations, product, pairwise

accumulate(nums)             # running prefix sums          O(n)
accumulate(nums, max)        # running max
pairwise(seq)                # adjacent pairs (3.10+)
product(range(3), repeat=2)  # flattened nested loops / bitmask generation
combinations(arr, 2)
permutations(arr)
chain(a, b)                  # lazily concatenate iterables

from functools import cache, lru_cache, reduce, cmp_to_key

@cache                       # top-down DP memoization (3.9+); lru_cache(None) pre-3.9
def dp(i, j):
    ...

sorted(items, key=cmp_to_key(my_cmp))   # custom 3-way comparator
```

# PART II — NeetCode / LeetCode Patterns

Each: **Cue** (when to reach for it) · template · complexity · representative problems.

## 1. Arrays & Hashing

**Cue:** counts, dedup, "have I seen this?". **O(n) time / O(n) space.**

```python
seen = {}

for i, x in enumerate(nums):
    if target - x in seen:
        return [seen[target - x], i]
    seen[x] = i
```

*Two Sum, Group Anagrams, Top-K Frequent, Contains Duplicate, Valid Anagram, Product of Array Except Self, Valid Sudoku, Longest Consecutive Sequence.*

## 2. Two Pointers

**Cue:** sorted input; pair/triplet; palindrome; in-place dedup. **O(n) after sort.**

```python
l, r = 0, len(a) - 1

while l < r:
    s = a[l] + a[r]
    if s == target:
        return (l, r)
    if s < target:
        l += 1
    else:
        r -= 1
```

*Valid Palindrome, Two Sum II, 3Sum, Container With Most Water, Trapping Rain Water.*

## 3. Sliding Window

**Cue:** longest/shortest contiguous subarray or substring meeting a constraint. **O(n).**

```python
l = 0
window = {}

for r, c in enumerate(s):
    window[c] = window.get(c, 0) + 1

    # shrink from the left until the window is valid again
    while violates(window):
        window[s[l]] -= 1
        l += 1

    best = max(best, r - l + 1)
```

Fixed-size variant: slide a width-k frame, add `nums[r]`, drop `nums[r-k]`. *Best Time to Buy/Sell Stock, Longest Substring Without Repeating, Longest Repeating Char Replacement, Permutation in String, Minimum Window Substring, Sliding Window Maximum (monotonic deque).*

## 4. Stack / Monotonic Stack

**Cue:** matching/balancing; "next greater/smaller"; histogram spans. **O(n).**

```python
stack = []   # holds indices, values increasing

for i, x in enumerate(nums):
    while stack and nums[stack[-1]] < x:
        j = stack.pop()
        ans[j] = i - j        # distance to next greater
    stack.append(i)
```

*Valid Parentheses, Min Stack, Evaluate RPN, Generate Parentheses, Daily Temperatures, Car Fleet, Largest Rectangle in Histogram.*

## 5. Binary Search (+ search-on-answer)

**Cue:** sorted array; OR "minimize the max / maximize the min / find a threshold". **O(log n)** or **O(n log range).**

```python
lo, hi = min_ans, max_ans     # search on the answer space

while lo < hi:
    mid = (lo + hi) // 2
    if feasible(mid):
        hi = mid
    else:
        lo = mid + 1

return lo
```

*Binary Search, Search a 2D Matrix, Koko Eating Bananas, Find Minimum in Rotated, Search in Rotated, Time-Based Key-Value Store, Median of Two Sorted Arrays.*

## 6. Linked List

**Cue:** cycle, middle, reorder, merge, k-group reversal. **O(n) / O(1) space.**

```python
# in-place reversal
prev = None
while head:
    head.next, prev, head = prev, head, head.next

# fast/slow for cycle or middle
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
```

*Reverse Linked List, Merge Two Lists, Linked List Cycle, Reorder List, Remove Nth From End, Copy List w/ Random Pointer, Add Two Numbers, LRU Cache, Merge K Sorted Lists, Reverse Nodes in k-Group.*

## 7. Trees

**Cue:** traversal, path/depth, BST invariant. **O(n).**

```python
# depth / path aggregation
def dfs(node):
    if not node:
        return 0
    return 1 + max(dfs(node.left), dfs(node.right))

# BFS level-order
q = deque([root])
while q:
    for _ in range(len(q)):
        n = q.popleft()
        for c in (n.left, n.right):
            if c:
                q.append(c)
```

*Invert Tree, Max Depth, Diameter, Balanced Tree, Same/Subtree, LCA of BST, Level Order, Right Side View, Count Good Nodes, Validate BST, Kth Smallest in BST, Construct from Preorder/Inorder, Serialize/Deserialize.*

## 8. Tries

**Cue:** prefix search, word dictionary, autocomplete. **O(L) per op.**

```python
root = {}

for w in words:                      # insert
    node = root
    for ch in w:
        node = node.setdefault(ch, {})
    node['$'] = True                 # end-of-word marker
```

*Implement Trie, Design Add and Search Words, Word Search II.*

## 9. Backtracking

**Cue:** "all subsets/permutations/combinations"; constraint satisfaction. **Exponential, pruned.**

```python
def bt(start, path):
    res.append(path[:])              # record (or check goal)
    for i in range(start, n):
        if invalid(i):
            continue                 # prune
        path.append(nums[i])
        bt(i + 1, path)
        path.pop()                   # undo
```

*Subsets (I/II), Combination Sum (I/II), Permutations, Word Search, Palindrome Partitioning, Letter Combinations, N-Queens.*

## 10. Heap / Priority Queue / Top-K

**Cue:** k largest/smallest; merge-k; streaming median; schedule by frequency. **O(n log k).**

```python
# two-heap running median (small = max-heap, large = min-heap)
small, large = [], []

def add(x):
    heapq.heappush(small, -heapq.heappushpop(large, x))
    if len(small) > len(large):
        heapq.heappush(large, -heapq.heappop(small))
```

*Kth Largest in a Stream, Last Stone Weight, K Closest Points, Kth Largest Element (or quickselect), Task Scheduler, Design Twitter, Find Median from Data Stream.*

## 11. Graphs (basic)

**Cue:** grid islands, connectivity, components, cycle detection. **O(V + E).**

```python
# Union-Find with path compression
def find(x):
    while x != par[x]:
        par[x] = par[par[x]]
        x = par[x]
    return x

def union(a, b):
    ra, rb = find(a), find(b)
    if ra == rb:
        return False
    par[ra] = rb
    return True
```

*Number of Islands, Max Area of Island, Clone Graph, Pacific Atlantic, Surrounded Regions, Rotting Oranges, Walls and Gates, Course Schedule (I/II), Redundant Connection, Number of Connected Components, Graph Valid Tree, Word Ladder.*

## 12. Advanced Graphs

**Cue:** weighted shortest path, dependency ordering, min spanning tree. **O(E log V).**

```python
# Dijkstra
pq = [(0, src)]
dist = {}

while pq:
    d, u = heapq.heappop(pq)
    if u in dist:
        continue
    dist[u] = d
    for v, w in adj[u]:
        if v not in dist:
            heapq.heappush(pq, (d + w, v))
```

Kahn's topo sort: BFS over nodes with indegree 0 using a deque. *Network Delay Time, Reconstruct Itinerary, Min Cost to Connect Points (MST), Swim in Rising Water, Alien Dictionary (topo), Cheapest Flights within K Stops (Bellman-Ford).*

## 13. 1-D Dynamic Programming

**Cue:** "ways to"; max/min along a sequence; subsequence. **O(n) or O(n·target).**

```python
# House Robber shape
dp = [0] * (n + 1)

for i in range(1, n + 1):
    dp[i] = max(dp[i - 1], dp[i - 2] + val[i])
```

*Climbing Stairs, House Robber (I/II), Longest Palindromic Substring, Palindromic Substrings, Decode Ways, Coin Change, Maximum Product Subarray, Word Break, Longest Increasing Subsequence, Partition Equal Subset Sum.*

## 14. 2-D Dynamic Programming

**Cue:** two strings; grid paths; knapsack; interval DP. **O(m·n).**

```python
# Longest Common Subsequence
dp = [[0] * (n + 1) for _ in range(m + 1)]

for i in range(1, m + 1):
    for j in range(1, n + 1):
        if a[i - 1] == b[j - 1]:
            dp[i][j] = dp[i - 1][j - 1] + 1
        else:
            dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
```

Interval DP (solve inner ranges first): the family of **Burst Balloons** and the card-picking game — iterate by length, then by left endpoint, choosing the partition point. *Unique Paths, Longest Common Subsequence, Buy/Sell with Cooldown, Coin Change II, Target Sum, Interleaving String, Longest Increasing Path in Matrix, Distinct Subsequences, Edit Distance, Burst Balloons, Regular Expression Matching.*

## 15. Greedy

**Cue:** intervals; jumps; "minimum number of"; local choice provably global. **O(n log n) (sort).**

*Maximum Subarray (Kadane), Jump Game (I/II), Gas Station, Hand of Straights, Merge Triplets to Form Target, Partition Labels, Valid Parenthesis String.*

## 16. Intervals

**Cue:** meetings, merge, overlap counting. **O(n log n).**

```python
intervals.sort()
merged = []

for s, e in intervals:
    if merged and s <= merged[-1][1]:
        merged[-1][1] = max(merged[-1][1], e)
    else:
        merged.append([s, e])
```

Min meeting rooms: min-heap of end times. *Insert Interval, Merge Intervals, Non-overlapping Intervals, Meeting Rooms (I/II), Minimum Interval to Include Each Query.*

## 17. Bit Manipulation

**Cue:** single number; count bits; subset enumeration via bitmask. **O(n) or O(n·bits).**

```python
x ^= num            # XOR cancels pairs
count += n & 1      # inspect lowest bit
n >>= 1             # shift right
n &= (n - 1)        # drop the lowest set bit
```

*Single Number, Number of 1 Bits, Counting Bits, Reverse Bits, Missing Number, Sum of Two Integers, Reverse Integer.*

## 18. Math & Geometry

**Cue:** in-place matrix ops; overflow handling; gcd; prefix products.

*Rotate Image, Spiral Matrix, Set Matrix Zeroes, Happy Number, Plus One, Pow(x, n), Multiply Strings, Detect Squares.*

# PART III — Python Language Constructs

## 1. Control Flow

```python
if x > 0:
    ...
elif x < 0:
    ...
else:
    ...

val = a if cond else b                 # ternary

for i, x in enumerate(arr, start=0):   # index + value
    ...
for a, b in zip(xs, ys):               # parallel iterate
    ...
for k, v in d.items():
    ...
for i in range(n - 1, -1, -1):         # reverse range
    ...

while l < r:
    ...

for x in seq:                          # loop-else: runs only if NO break
    if found(x):
        break
else:
    handle_not_found()

match cmd:                             # structural pattern matching (3.10+)
    case "start":
        ...
    case [a, b]:                       # sequence destructure
        ...
    case {"type": t}:                  # mapping match
        ...
    case _:                            # default
        ...
```

## 2. Comprehensions

```python
squares = [x * x for x in nums]
evens   = [x for x in nums if x % 2 == 0]
grid    = [[0] * cols for _ in range(rows)]   # 2D (not [[0]*cols]*rows)
pairs   = {k: v for k, v in items}            # dict comp
uniq    = {x for x in nums}                   # set comp
gen     = (x * x for x in nums)               # lazy generator expr
flat    = [c for row in matrix for c in row]  # nested (outer loop first)
```

## 3. Functions

```python
def f(a, b=0, *args, **kwargs):        # positional, default, varargs, kwargs
    ...

def g(acc=None):                       # AVOID mutable default (acc=[] persists)
    acc = acc if acc is not None else []

square = lambda x: x * x               # anonymous
q, r = divmod(17, 5)                   # multiple returns via tuple unpack

def make_counter():                    # closure + nonlocal
    count = 0
    def inc():
        nonlocal count
        count += 1
        return count
    return inc
```

## 4. Error Handling

```python
try:
    risky()
except (ValueError, KeyError) as e:    # catch specific, grouped
    handle(e)
except Exception as e:
    log(e)
    raise                              # re-raise same exception
else:
    on_success()                       # runs only if no exception
finally:
    cleanup()                          # always runs

raise ValueError("bad input")
raise RuntimeError("wrap") from e      # exception chaining

class TaskNotFound(Exception):         # custom exception
    pass

assert n >= 0, "n must be non-negative"
```

| Exception | Raised when |
|---|---|
| `IndexError` | list/sequence index out of range |
| `KeyError` | missing dict key |
| `ValueError` | right type, bad value (e.g. `int("x")`) |
| `TypeError` | wrong type / bad operands |
| `StopIteration` | iterator exhausted (next past end) |
| `ZeroDivisionError` | `/` or `%` by zero |
| `AttributeError` | missing attribute/method |
| `RecursionError` | recursion depth exceeded (~1000) |

**EAFP > LBYL in Python:** prefer try/except over pre-checking (e.g. `try: d[k] except KeyError` rather than `if k in d`), except in tight loops where the exception path is hot.

## 5. Classes / OOP

```python
class Node:
    __slots__ = ("val", "nxt")         # no __dict__: less memory, faster attr
    def __init__(self, val, nxt=None):
        self.val = val
        self.nxt = nxt
    def __repr__(self):                # debug-friendly
        return f"Node({self.val})"
    def __lt__(self, other):           # enables heap / sort of objects
        return self.val < other.val
    def __eq__(self, other):
        return self.val == other.val
    def __hash__(self):                # define with __eq__ to use as set/dict key
        return hash(self.val)
```

**Dunder methods worth knowing:** `__init__`, `__repr__`/`__str__`, `__eq__`+`__hash__` (hashable key), `__lt__` (heap/sort), `__len__`, `__getitem__`/`__setitem__`, `__contains__` (the `in` operator), `__iter__`/`__next__`, `__call__`.

```python
class Account:
    bank = "Acme"                      # class attr (shared across instances)
    def __init__(self, bal):
        self.bal = bal                 # instance attr
    @property
    def doubled(self):                 # computed, accessed as attr (no parens)
        return self.bal * 2
    @classmethod
    def from_cents(cls, c):            # alternate constructor
        return cls(c / 100)
    @staticmethod
    def fee(x):                        # no self / cls
        return x * 0.01

class Savings(Account):                # inheritance
    def __init__(self, bal, rate):
        super().__init__(bal)
        self.rate = rate
```

`dataclass` auto-generates init/repr/eq, and order/lt with `order=True`:

```python
from dataclasses import dataclass, field

@dataclass(order=True)
class Point:
    x: int
    y: int = 0
    tags: list = field(default_factory=list)   # mutable default, done safely
```

Abstract base classes & Enum (your Strategy-pattern / sealed-interface designs map here):

```python
from abc import ABC, abstractmethod

class PricingStrategy(ABC):
    @abstractmethod
    def price(self, ctx) -> float:
        ...

from enum import Enum

class Status(Enum):
    OPEN = 1
    CLOSED = 2
```

## 6. Iterators & Generators

```python
def squares(n):
    for i in range(n):
        yield i * i                    # lazy: produces one at a time

yield from other_gen()                 # delegate to a sub-generator

class CountUp:                         # manual iterator protocol
    def __init__(self, n):
        self.i, self.n = 0, n
    def __iter__(self):
        return self
    def __next__(self):
        if self.i >= self.n:
            raise StopIteration
        self.i += 1
        return self.i - 1
```

## 7. Decorators

```python
from functools import wraps, cache

def logged(fn):
    @wraps(fn)                         # preserves name / docstring
    def wrapper(*args, **kwargs):
        return fn(*args, **kwargs)
    return wrapper

@cache                                 # memoize -> top-down DP
def fib(n):
    return n if n < 2 else fib(n - 1) + fib(n - 2)
```

## 8. Context Managers

```python
with open("file.txt") as f:            # auto-closes even on exception
    data = f.read()

from contextlib import contextmanager

@contextmanager
def managed(res):
    acquire(res)
    try:
        yield res
    finally:
        release(res)                   # guaranteed cleanup
```

## 9. Essential Built-ins & Idioms

```python
enumerate(seq, start)        # index + value
zip(a, b)                    # parallel iterate
reversed(seq)
sorted(seq, key=fn, reverse=True)
min(seq, key=fn, default=0)
max(seq, key=fn, default=0)
any(conds)                   # short-circuits
all(conds)
sum(seq, start)
map(fn, seq)
filter(fn, seq)

a, b = b, a                  # swap, no temp
first, *rest = seq           # extended unpacking
*init, last = seq

INF, NINF = float('inf'), float('-inf')
val = next(it, sentinel)     # safe next, no StopIteration

if (m := compute()) > 0:     # walrus := assigns inside an expression (3.8+)
    use(m)

import sys
sys.setrecursionlimit(10**6)  # deep DFS
```

## 10. Type Hints (3.9+ style)

```python
def topk(xs: list[int], k: int) -> list[int]:
    ...

def find(d: dict[str, int], k: str) -> int | None:   # union / optional
    ...

from typing import Optional, Callable    # Optional[int] == int | None
handler: Callable[[int], bool]
```

## 11. Python Gotchas (interview-critical)

- **Mutable default args** persist across calls — use a `None` sentinel, then assign inside.
- **Late-binding closures:** `[lambda: i for i in range(3)]` all return 2. Bind early: `lambda i=i: i`.
- **`is` vs `==`:** `is` is identity; use it only for `is None`. Compare values with `==`.
- **Shallow copy:** nested lists share references; use `copy.deepcopy` for independent nesting.
- **Floor division:** `//` rounds toward −∞ (`-7 // 2 == -4`); use `int(a / b)` to truncate.
- **defaultdict** read access creates the key — use plain `dict.get` when you must not mutate.
- **Recursion limit ~1000** — raise it or convert deep recursion to iterative with an explicit stack.
- **String concat in a loop** is O(n²) — collect into a list and `"".join(...)`.
