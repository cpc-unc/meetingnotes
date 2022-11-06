# Problem 1 Solution: [sqrt(x)](https://leetcode.com/problems/sqrtx/)

Given a number `x`, we want to find its square root, without using any built-in square root functions. Instead of testing $1^2$, $2^2$, $3^2$, ..., all the way up to $x^2$, we can use a binary search algorithm. We'll test this algorithm out with a sample number of `x` = 49.

- First, we take the number exactly between 1 and 49. We find the midpoint, which is 20, and square this (which is much larger than 49). So we know the answer must be between 1 and 20.
- Then we take the midpoint of those two, which gives us 10, which is also greater than $\sqrt{49}$, so we take the average of 1 and 10 again, which gives us 5.
- Computing $5^2$ gives us 25, which is now less than 49. So we can change our lower bound to 6. Now, we take the average of 6 and 10, which is 7, which is exactly $\sqrt{49}$. We've now found our answer.

Here's how we can set up our binary search algorithm for this problem.

```python
def mySqrt(x):
	start = 1
	end = x
	while(start < end):
		mid = (int)((start+end)/2)

		# Test to see if this midpoint is less than, greater than, or equal to our answer.

	return start
```

So within this test, what do we do? If it's less than our answer, then we want `start` to equal `mid`. For example, if we're testing for the square root of 49 on the number 5 (average of 1 and 10), $5 < \sqrt{49}$, so we need to shift our endpoints to 6 and 10.

Alternatively, if our guess squared is greater than 49, we need to adjust our `end` point to `mid` (for example, $20^2 > 49$, so the `end` point goes from 49 to 20).

## Answer

```python
def mySqrt(x):
	start = 1
	end = x
	while(start < end):
		mid = (int)((start+end)/2)

		if(mid**2 < x):
			# mid is too small of a value!
			start = mid + 1
		else:
			# mid is too large (or equal to)
			end = mid

	return start
```

Why do we add 1 to start with the line `start = mid + 1`, but don't account for that symmetry by doing `end = mid - 1`?
Think about when `start` = 3, and `end` = 4. We'll leave this corner case for you to think about.