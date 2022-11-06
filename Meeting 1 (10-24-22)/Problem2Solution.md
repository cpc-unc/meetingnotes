# Problem 2 solution - [Flower bouquet problem](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)

## NOTE: this code is made like a pseudocode solution to guide you to understand how to solve the problem; it is not guaranteed to work.

We want to figure out the **minimum** number of days required for `m` bouquets of `k` adjacent flowers to bloom. Again we use a binary search algorithm to efficiently figure out what number of days it will bloom at. __If you don't have a good understanding of the binary search algorithm, please check the Problem 1 solution in Meeting 1__.

First, we find our endpoints. The minimum possible answer is 1, and the maximum possible answer is the maximum of the `bloomDay` array.

```python
def minDays(bloomDay, m, k):
	start = 1
	end = max(bloomDay)
```

Then, we use our template binary search algorithm to find the halfway point between `start` and `end`, and test the code on that.

We need to make a method `checkBouquets(n, bloomDay, m, k)` to determine whether given `n` days for flowers to bloom, do we have `m` bouquets that have `k` consecutive flowers in it? This method should return `true` if we do, and `false` if we don't.

```python
def minDays(bloomDay, m, k):
	start = 1
	end = max(bloomDay)

	while(start < end):
		mid = (int)((start + end)/2)

	if checkBouquets(mid, bloomDay, m, k):
		end = mid
	else:
		start = mid + 1
```

Why do we reduce our `end` point when `checkBouquets(mid, bloomDay, m, k)` is true? That's because if `mid` days gives us enough bouquets, we **do not need any more days**. In fact, we should test to see if we can do with **less days**, since we're trying to find a minimum number of days such that `checkBouquets(mid, bloomDay, m, k)` is true.
Alternatively, if `checkBouquets(mid, bloomDay, m, k)` is false, then we need more days, so we push the `start` value up to `mid`.

Now we need to figure out how to implement the `checkBouquets(mid, bloomDay, m, k)` method.

Let's have a counter for how many consecutive flowers we have found. If `bloomDay[i] <= n`, then the flower at the `i`-th position needs <= `n` days to bloom, so we mark it as a bloomed flower.

```python
def checkBouquets(n, bloomDay, m, k):
    consecutiveCounter = 0
	for i in range(bloomDay):
		if bloomDay[i] <= n:
			consecutiveCounter += 1
        else:
            consecutiveCounter = 0
```

Now we need to check whether we have a completed bouquet.
```python
def checkBouquets(n, bloomDay, m, k):
    consecutiveCounter = 0
    numBouquets = 0
	for i in range(bloomDay):
		if bloomDay[i] <= n:
			consecutiveCounter += 1
        else:
            consecutiveCounter = 0
        
        if consecutiveCounter == k:
            numBouquets += 1
            consecutiveCounter = 0
```

When we reach `k` consecutive flowers, we have a completed bouquet, so we reset the `consecutiveCounter` and add a bouquet to `numBouquets`.

Finally, we need to check whether we have `m` bouquets or not, so we have:

```python
def checkBouquets(n, bloomDay, m, k):
    consecutiveCounter = 0
    numBouquets = 0
	for i in range(bloomDay):
		if bloomDay[i] <= n:
			consecutiveCounter += 1
        else:
            consecutiveCounter = 0
        
        if consecutiveCounter == k:
            numBouquets += 1
            consecutiveCounter = 0
    return numBouquets >= m
```