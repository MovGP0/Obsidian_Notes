## Sorting Algorithms

- **Quick Sort**: A divide-and-conquer algorithm that selects a pivot, partitions the array into elements less than and greater than the pivot, and recursively sorts each partition. Average time complexity $O(n \log n)$.
- **Merge Sort**: A stable divide-and-conquer algorithm that recursively splits the array in half, sorts each half, and merges them in $O(n \log n)$ time.
- **Heap Sort**: Builds a binary max-heap from the data and repeatedly extracts the maximum element to sort the array in $O(n \log n)$ time; not stable but in-place.
- **Counting Sort**: Non-comparative integer sorting that counts occurrences of each key and computes positions via prefix sums in $O(n + k)$ time, where k is the key range.
- **Radix Sort**: Processes integer keys digit by digit (least or most significant) using a stable sort (often counting sort) per digit, achieving $O(n · k)$ time for k-digit keys.

## Searching Algorithms

- **Linear Search**: Sequentially checks each element until it finds the target or reaches the end; $O(n)$ time, simple to implement.
- **Binary Search**: On sorted arrays, repeatedly halves the search interval to locate a target in $O(\log n)$ time.
- **Hash-Based Search**: Uses a hash function to map keys to buckets for average-case $O(1)$ lookup, insertion, and deletion.

## Graph Algorithms

- **Breadth-First Search (BFS)** and **Depth-First Search (DFS)**: Traverse or search graph vertices layer by layer (BFS) or by exploring as far as possible along each branch (DFS) in $O(V + E)$ time.
- **Dijkstra’s Algorithm**: Computes shortest paths from a source to all vertices in a weighted graph with non-negative weights in $O((V + E) \log V)$ time using a priority queue.
- **Prim’s and Kruskal’s Algorithms**: Find a minimum spanning tree of a connected weighted graph using greedy edge selection; both run in $O(E \log V)$ time.

## Dynamic Programming

- **General DP**: Breaks problems with overlapping subproblems and optimal substructure into smaller subproblems, solves each once, and stores results to avoid recomputation (configr.medium.com).
	- **Examples**: Longest Common Subsequence, 0-1 Knapsack, Edit Distance – typical DP problems solvable in polynomial time by tabulation or memoization.

## Greedy Algorithms

- **Greedy Paradigm** Builds a solution iteratively by choosing the locally optimal option at each step, yielding a global optimum for certain problems.
- **Huffman Coding** Constructs an optimal prefix code for data compression by repeatedly merging the two lowest-frequency nodes.

## Divide and Conquer

- **Divide & Conquer**: Splits a problem into independent subproblems, solves them recursively, and combines their solutions.
  Foundations of Quick Sort, Merge Sort, and Fast Fourier Transform.

## Backtracking

- **Backtracking**: Explores solution spaces recursively, abandoning (“backtracking”) paths that cannot lead to valid solutions; used in N-Queens, Sudoku, and combinatorial search.

## Number-Theoretic Algorithms

- **Sieve of Eratosthenes**: Finds all primes up to n by iteratively marking multiples of each prime, running in $O(n \log \log n)$ time.
- **Euclidean Algorithm**: Computes the greatest common divisor (GCD) of two integers via repeated remainder operations in $O(\log \min(a,b))$ time.

## String Algorithms

- **Knuth–Morris–Pratt (KMP)**: Performs substring search in $O(n + m)$ time by precomputing a failure function to skip comparisons.
- **Rabin–Karp**: Uses rolling hash to detect pattern matches in expected $O(n + m)$ time, with possible false positives.
- **Aho–Corasick**: Builds a finite automaton for multiple pattern matching in $O(n + ∑|\text{pattern}| + z)$ time, where z is the number of matches.

## Computational Geometry

- **Graham Scan**: Computes the convex hull of a planar point set in $O(n \log n)$ time by sorting points by angle and scanning.
- **Jarvis March (Gift Wrapping)**: Finds the convex hull by “wrapping” points in $O(nh)$ time, where h is the number of hull vertices.
- **Quickhull**: A divide-and-conquer convex hull algorithm with expected $O(n \log n)$ time.

## Data-Structure-Based Queries

- **Segment Tree**: Supports range queries and updates on arrays in $O(\log n)$ per operation by building a binary tree of intervals.
- **Fenwick Tree (Binary Indexed Tree)**: Provides prefix-sum queries and point updates in $O(\log n)$ time with a compact array structure.
- **Union-Find (Disjoint Set Union)**: Maintains disjoint sets with near-constant-time union and find operations via path compression and union by rank.
- **Trie (Prefix Tree)**: Stores strings by shared prefixes for $O(m)$ lookup, insertion, and deletion of an m-character string.
- **Suffix Array / Suffix Tree**: Enables fast substring queries, pattern matching, and string statistics in $O(n)$ to $O(n \log n)$ preprocessing and $O(m)$ query time.

## See also

- [Advanced Algorithms](https://github.com/justcoding121/Advanced-Algorithms)
