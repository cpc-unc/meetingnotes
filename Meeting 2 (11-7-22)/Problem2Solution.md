# Problem 2 Solution: [Optimal path](https://codeforces.com/problemset/problem/1700/A)

This is a greedy constructive algorithm. We first start with the top left square, and then go to every square that is possible with 2 moves. Then we go to every square that's possible with 3 moves, and so on, until we hit the bottom right square.

Let's say we have a 4 by 8 square, where $n$ = 4, and $m$ = 8.

For **step 1**, we start at the top left square.

⬜⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛

For **step 2**, we can go to either of the white squares, as we can move one to the right or one down.

⬛⬜⬛⬛⬛⬛⬛⬛<br>
⬜⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛

For **step 3**, our possible destinations are:

⬛⬛⬜⬛⬛⬛⬛⬛<br>
⬛⬜⬛⬛⬛⬛⬛⬛<br>
⬜⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛

And for **step 4**:

⬛⬛⬛⬜⬛⬛⬛⬛<br>
⬛⬛⬜⬛⬛⬛⬛⬛<br>
⬛⬜⬛⬛⬛⬛⬛⬛<br>
⬜⬛⬛⬛⬛⬛⬛⬛

**Step 5**:

⬛⬛⬛⬛⬜⬛⬛⬛<br>
⬛⬛⬛⬜⬛⬛⬛⬛<br>
⬛⬛⬜⬛⬛⬛⬛⬛<br>
⬛⬜⬛⬛⬛⬛⬛⬛

**Step 6**:

⬛⬛⬛⬛⬛⬜⬛⬛<br>
⬛⬛⬛⬛⬜⬛⬛⬛<br>
⬛⬛⬛⬜⬛⬛⬛⬛<br>
⬛⬛⬜⬛⬛⬛⬛⬛

**Step 7**:

⬛⬛⬛⬛⬛⬛⬜⬛<br>
⬛⬛⬛⬛⬛⬜⬛⬛<br>
⬛⬛⬛⬛⬜⬛⬛⬛<br>
⬛⬛⬛⬜⬛⬛⬛⬛

**Step 8**:

⬛⬛⬛⬛⬛⬛⬛⬜<br>
⬛⬛⬛⬛⬛⬛⬜⬛<br>
⬛⬛⬛⬛⬛⬜⬛⬛<br>
⬛⬛⬛⬛⬜⬛⬛⬛

**Step 9**:

⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬜<br>
⬛⬛⬛⬛⬛⬛⬜⬛<br>
⬛⬛⬛⬛⬛⬜⬛⬛

**Step 10**:

⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬜<br>
⬛⬛⬛⬛⬛⬛⬜⬛

**Step 11** (final step):

⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬜

So how many steps are there, given the variables $n$ and $m$? Let's try to figure this out.


## Finding out how many steps there are

First, looking **just at the top row**, we know that there are always going to be exactly $m$ (in our case, 8) steps that move the white diagonal line from the left to the right, from **step 1** to **step 8**.

Step 1&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Step 8<br>
⬜⬛⬛⬛⬛⬛⬛⬛&emsp;&emsp;⬛⬛⬛⬛⬛⬛⬛⬜<br>
⬛⬛⬛⬛⬛⬛⬛⬛&emsp;&emsp;⬛⬛⬛⬛⬛⬛⬜⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛&emsp;&emsp;⬛⬛⬛⬛⬛⬜⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬛⬛&emsp;&emsp;⬛⬛⬛⬛⬜⬛⬛⬛


## Phase 2

Notice at step $m$, there are exactly $n$ squares remaining (one for each row). And since each square is 1 to the left of the one above it, the last square (square $n$, or in this case, square 4) is going to be exactly $n-1$ places away from the bottom right square (our destination). So now, we need $n-1$ steps to reach from **step $m$** to the last step.

Step 8&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Step 11<br>
⬛⬛⬛⬛⬛⬛⬛⬜&emsp;&emsp;⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬛⬜⬛&emsp;&emsp;⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬛⬜⬛⬛&emsp;&emsp;⬛⬛⬛⬛⬛⬛⬛⬛<br>
⬛⬛⬛⬛⬜⬛⬛⬛&emsp;&emsp;⬛⬛⬛⬛⬛⬛⬛⬜

**Therefore, we need exactly $m+n-1$ steps to go through the entire matrix.**

## Iterating through each step

For each `step`, we want to go through all possibilities of destinations we can end up in.

```python
def solve(m, n):
    grid = []

    # This creates a grid that has n arrays with m elements each. i.e. an n by m 2D array.
    for i in range(n):
        x = []
        for j in range(m):
            x.append(0)
        grid.append(x)

    for step in range(n+m-1):
        # Run algorithm on this specific step
```

Notice that the `step` variable iterates from $0$ to $m+n-2$.

Given `step` number of steps, what are all the possibilities for places we could end up at on the board? Well, we could go all the way to the right, with `grid[0][step]` or all the way down, with `grid[step][0]`, or anywhere in between (some combination of rights and downs). In particular, we can land on any square `grid[a][b]` where `a + b = step`.

To make this algorithm efficient, we need to figure out the endpoints/bounds. `a` can iterate from 0 all the way to `n - 1` (as this is the max height of the grid), or it can iterate from 0 all the way to `step`, depending on which one is closer.

Likewise, `b` can iterate from 0 all the way to `m - 1`. If we use the equation $s = a + b$ (where $s$ is `step`, just in LaTeX), then we can make tighter bounds for `a`.

For the lower bound for `b`, or upper bound for `a`:

**$\textbf{b = 0}$**<br>
$a = s$
<br>

This means `a` can either end at `n - 1`, or `s`, whichever one is smaller.

For the upper bound for `b`, or lower bound for `a`:

**$\textbf{b = m - 1}$**<br>
$s = a + m - 1$<br>
$a = s - m + 1$

This means `a` can either start at `0`, or `s - m + 1`, whichever one is greater.

```python
def solve(m, n):
    grid = []

    # This creates a grid that has n arrays with m elements each. i.e. an n by m 2D array.
    for i in range(n):
        x = []
        for j in range(m):
            x.append(0)
        grid.append(x)

	for step in range(n+m-1):
		for a in range(max(step-m+1, 0), min(step, n-1) + 1): # Using the upper/lower bounds for a we just found
			# Run the rest of the algorithm here
            # on grid[a][step-a]
```

So for the rest of the algorithm, we need to differentiate between the **square cost**, which is just a numbered cost $1, 2, 3, ..., mn$, and the **minimum arrival cost (MAC)** is the minimum cost needed to arrive at a certain square. The objective of the algorithm is to find the **MAC** of `grid[n-1][m-1]`.

To do this, we need to calculate the a way to calculate the MAC of every square. The cost of staying at the first square, is 1. The cost of any square `grid[a][step-a]` is `a*m + step - a + 1` (see the problem description).

The MACs are going to be conveniently stored in the `grid` variable. So if we move from the square directly above into the current square (`grid[a][step-a]`), then the MAC of `grid[a][step-a]` is `grid[a-1][step-a] + a*m + step - a + 1` (the MAC of the square above plus the square cost of our current square).

Similarly, if we move from the square directly to the left of our current square (`grid[a][step-a]`), then the MAC of `grid[a][step-a]` is `grid[a][step-a-1] + a*m + step - a + 1` (the MAC of the square to the left plus the square cost of our current square).

We need to compare these two MACs to see which one is cheaper, and store that in our current cell value (`grid[a][step-a]`). Remember we'll still need to check whether or not the square above and to the left exists. And we need to initialize the first cell to have a MAC of 1.

```python
def solve(m, n):
    grid = []

    # This creates a grid that has n arrays with m elements each. i.e. an n by m 2D array.
    for i in range(n):
        x = []
        for j in range(m):
            x.append(0)
        grid.append(x)

    grid[0][0] = 1 # Establish first MAC to be 1.

	for step in range(n+m-1):
		tgrid = []

		for i in range(n):
			x = []
			for j in range(m):
				x.append(0)
			tgrid.append(x)
		
		for a in range(max(step-m+1, 0), min(step, n-1) + 1): # Using the upper/lower bounds for a we just found
			leftMAC = float('inf')
			upMAC = float('inf')

			if a - 1 >= 0:
				# the square above exists
				upMAC = grid[a-1][step-a]
			if step-a - 1 >= 0:
				# the square to the left exists
				leftMAC = grid[a][step-a-1]

			if step != 0:
				grid[a][step-a] = min(leftMAC + a*m + step - a + 1, upMAC + a*m + step - a + 1)

			tgrid[a][step-a] = a*m + step - a + 1
    
    return grid[n-1][m-1]
```

We take care of the case where `step = 0`, because this is when `leftMAC` and `rightMAC` equals infinity.

And this should output exactly what we want: the MAC of the last square in our grid.