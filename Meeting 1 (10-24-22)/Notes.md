# Meeting 1 (10/24/22)

[Slides](https://docs.google.com/presentation/d/11KYorPdocJ7se_TmDYFsR7VkXmLpuq7OIsI7AS6Y8Ks/edit?usp=share_link) for meeting 1

## Binary search

- Iteratively divide subarrays in half to search for element
- Assumes array is sorted
- Can generalize algorithm to search on ranges without explicit array data structure
- Runs efficiently, $O(n log(n))$

## Basic implementation of binary search:

```
l = 0, r = N
while (l < r):
	mid = (l+r)/2
	if (condition(mid)):
		l = mid+1
	else:
		r = mid
return l
```

- Use library `sort` to preprocess array
- Tweak code to go from right to left, etc.
- C++ has `std::lower_bound` and `std::upper_bound`
- Need to `include <algorithm>`
- Python has `bisect.bisect_left` and `bisect.bisect_right`
- Need to import `bisect`

## Problems we went over

- Problem 1 - [sqrt(x)](https://leetcode.com/problems/sqrtx/) (Leetcode problem)
- Problem 2 - [Flower bouquet problem](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/) (Leetcode problem)
- Problem 3 - [Sagheer and Nubian market](https://codeforces.com/problemset/problem/812/C) (Codeforces)