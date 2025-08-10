+++
date = '2025-08-09T13:18:02+01:00'
draft = true
title = 'Greedy Algorithms'
summary = 'Constructing solutions by making locally optimal choices'
toc = true
math = true
+++
### Greedy Algorithms

A greedy algorithms constructs a solution to the problem by making the best
choice at the time. That's the greedy part. Instead of looking for a "globally
optimal" solution, a greedy algorithm picks the best possible choice each step,
which can very often approximate the optimal solution. It does not take back its
choices but goes directly to construct the final solution.

The difficulty in designing these algorithms is to find a greedy strategy to
produce the best solution.

#### Coin Problem
Like the number sum problem we saw before, this is a problem where we need to
get a certain sum from a list of values, and additionally say how many coins
that would take. For example:

Given: ${1,2,5,10,20,50,100,200}$
and $n =  250$, we need at least 4 coins, 200+200+100+20.