+++
date = '2025-08-09T13:19:33+01:00'
draft = true
title = 'Complete Search'
summary = 'Brute forcing your way to the truth.'
toc = true
math = true
+++
### Complete Search

Complete search is a general method that can be used to solve almost any
algorithmic problem. It works by generating all the possible solutions and then
selecting the best solution or counting the solutions. Obviously in terms of
efficiency this is pure ass.

You can do this more efficiently with dynamic programming or greedy algorithms.
Of course, these are conceptually more difficult and harder to implement.

#### Generating subsets
You can generate subsets by recursion, or by using the bit representation of
integers. Each subset of n elements can be represented as a sequence of n bits.
between $0\dots 2^{n-1}$.

#### Generating Permutations
We can do this with recursion, or using standard lib function.

#### Backtracking
Backtracking algorithms begin with an empty solution and extend the solution
step by step, stopping when an incorrect or incomplete solution is found and
then backtracking to the last place.

For example, the problem of placing n queens on an nxn board can be solved this
way. We place one queen, then investigate every possibility at that level, and
do this by recursion. If we get an illegal placement or can't place n queens,
then we go back up.

Implementation below:
```cpp
void search(int y) {
    if (y == n) {
        count++;
        return;
    }
    for (int x = 0; x < n; x++) {
        if (column[x] || diag1[x+y] || diag2[x-y+n-1]) continue;
        column[x] = diag1[x+y] = diag2[x-y+n-1] = 1;
        search(y+1);
        column[x] = diag1[x+y] = diag2[x-y+n-1] = 0;
    }
}
```

Pruning the algorithms with intelligent choices can speed up runtime by orders
of magnitude, such as by adding code to halt the algorithm when it becomes clear
a solution cannot be reached on that "branch"

We can also add efficiencies by using "meet in the middle" - dividing the search
space into two parts and then performing the search, then combining the result.
This logic underlies the Merge Sort algorithm.

For example, if we are given a list of numbers and we want to know if it is
possible to get some sum of numbers from that list, we can use this.

Let our desired sum be 15, and our list be [2,4,5,9]. We can choose 2+4+9 to get
15, as requested.

If we want 10, this won't work. A simple algorithm to the problem is just to go
through all the subsets and check the sum, at $O(2^{n})$.

Splitting the list into two lists, then generating subsets and summing them for
both sides, checks if you can get a solution from just half the numbers. We'll
add those sums to our subsets to represent combinations. Then, we can go ahead
and check if we can pick an element from the one and then the other to get our
sum.

e.g. given [2,4,5,9], we can split into [2,4] and [5,9], then extend those with
0 and with the sums like so [0,2,4,6] and [0,5,9,14]. Taking combos of these 2
will generate the solution of 6+9, which corresponds to the solution [2,4,9]
