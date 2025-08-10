+++
date = '2025-08-09T13:19:56+01:00'
draft = true
title = 'Sorting'
summary = 'Putting everything in order.'
toc = true
math = true
+++
### Sorting

#### Bubble Sort
This is a very simple and famous sorting algorithm where the values "bubble up"
to the top. It requires 2 nested loops. It consists of n rounds, where on each
round, the algorithm switches the locations of every 2 elements where the latter
is larger than the earlier, essentially swapping every 2 consecutive elements to
be in the correct order. After this the total array will be in order.

```cpp
for (int i = 0; i < n; i++){
    for int (j = 0; j < n-1; j++) {
        if (array[j] > array[j+1]) {
            swap(array[j], array[j+1];
        }
    }
}
```

After the first round, the largest element will be in position. After k rounds,
the k largest elements will be in position. Hence, after n rounds, all will be
in order.

Because it requires 2 loops, operating over n rounds, with n swaps maximum
needed for a completely reverse ordered array, time complexity is $O(n^2)$

#### Merge Sort
How can we make our sorting algorithm more efficient? We can do this by not
limiting ourselves to consecutive elements being swapped. Merge sort does this,
by splitting the array into sub arrays, sorting those, then merging the result.

In more detail, mergesort sorts a subarray [a..b] like so:
1. If a = b, do nothing. It's already sorted!
2. Find the middle element $k = lower((a+b)/2)$
3. Recursively sort that subarray $[a...k]$
4. Recursively sort the other subarray $[k+1...b]$
5. Merge the sorted subarrays array $[a...k]$ and array $[k+1...b]$ into a
   sorted subarray $[a...b]$
   
Merge sort is efficient because it halves the size of the subarray at each step,
consisting of $O(n\log{n})$

An implementation is below: 
```cpp
#include <vector>

using namespace std;


vector<int> Merge(vector<int>& left, vector<int>& right) {
    vector<int> output;

    while (!left.empty() && !right.empty()) {
        int min_num = (left[0] <= right[0]) ? left[0] : right[0];
        output.push_back(min_num);

        if (left[0] <= right[0]) {
            left.erase(left.begin());
        } else {
            right.erase(right.begin());
        }
    }

    output.insert(output.end(), left.begin(), left.end());
    output.insert(output.end(), right.begin(), right.end());

    return output;
}

vector<int> MergeSort(vector<int>& arr) {
    int n = arr.size();

    if (n <= 1) {
        return arr;
    }

    int mid = n / 2;
    vector<int> left(arr.begin(), arr.begin() + mid);
    vector<int> right(arr.begin() + mid, arr.end());

    left = MergeSort(left);
    right = MergeSort(right);

    return Merge(left, right);
}
```
Time complexity is $O(n\log{n})$

#### Binary Search

This is a very efficient way of searching an array that relies on the array
being sorted. A main application of sorting data is to making searching more
efficient - think of a phone book or any other kind of directory.

Implementation below:
```cpp
int k = 0;
for (int b = n/2; b >= 1; b /= 2) {
    while (k+b < v && array[k+b] <= x) k += b;
}
if (array[k] == x){
    // x is found at k
}
```
What this does is use progressively smaller jumps, and checks at each jump if
the value at the jumped-to position is bigger or smaller than x. If x is bigger,
then we note the jumped position as k and jump again, adding our smaller jump to
k. If not, we jump from the start of the array, as we didn't do k += b. The idea
is that at each step the jump length is halved, and the variable k keeps track
of where we jumped to if x is larger, getting us to jump to the right of our
last jump, and if not, makes us jump into the sub array on the left.

Time complexity is $O(\log{n})$

