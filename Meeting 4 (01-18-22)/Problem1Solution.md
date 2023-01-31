# Problem 1 Solution - [Flood fill](https://leetcode.com/problems/flood-fill/)

## Try to attempt this problem yourself before you read the solution!

### Hint:

Think about what kind of traversal algorithm this is. Are we going to the bottom-most node, or the root (farthest number possible from the center) or are we traversing the closest nodes first?

It's the latter: we're starting from the center node, and going outwards from there.

Let's initialize the code with some essential values that we need. We need variables for the width/height of the image, as well as another *boolean* image that keeps track of which cells have been "painted". At the beginning, `image[sr][sc]` is painted, so the corresponding value in the `checked` image (`checked[sr][sc]`) is `true`, while every other value in this image is `false`.

```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        // Height of the image
        int m = image.size();
        // Width of the image
        int n = image[0].size();

        // Matrix that keeps track of painted cells
        vector<vector<bool>> checked;
        // Initialize all of the values in checked to be false
        for(int i = 0; i < m; i++) {
            vector<bool> temp(n, false);
            checked.push_back(temp);
        }
    }
};
```

Then we have to implement the classic BFS algorithm to run through the cells (which is in the slides):

```cpp
// THIS IS PSEUDOCODE. It will not compile.

queue q; 
bool visited[N]; 
int distance[N]; 

visited[x] = true;
distance[x] = 0;
q.push(x);
while (!q.empty()) {
    int s = q.front(); q.pop();
    // process node s
    for (auto u : adj[s]) {
        if (visited[u]) continue;
        visited[u] = true;
        distance[u] = distance[s]+1;
        q.push(u);
    }
}
```

And this is how we implement the queue in C++:

```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        int m = image.size();
        int n = image[0].size();
        vector<vector<bool>> checked;

        for(int i = 0; i < m; i++) {
            vector<bool> temp(n, false);
            checked.push_back(temp);
        }

        queue<pair<int, int>> q;

        // Like we said, this is the first value that's being "painted". So we put it at the top of the queue.
        q.push(make_pair(sr, sc));
        // This stores the value of the color that we started with
        int c = image[sr][sc];

        while(!q.empty()) {
            pair<int, int> next = q.front();
            q.pop();

            // nr, nc gets the row and column of the cell that is currently being processed
            int nr = next.first;
            int nc = next.second;

            // Process this node
        }
    }
};
```

If you noticed, there was one extra change that was added: the `c` variable stores the color that we start with. If a bunch of 1's are painted to turn into 2's, then `c` has the value 1, and `color` has the value 2.

So how do we process each individual cell that's popped off the queue? There's a couple of things we need to do:
1. Change the color of the cell. Actually paint it
2. Mark that cell as seen (change the `checked` cell to `true`)
3. Add the top, left, right, and bottom cells to the queue
    * Check if they exist
    * Check if they have already been painted
    * Check if the color at that position is the color that we started with

Here's how to do the first 2 steps.

```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        int m = image.size();
        int n = image[0].size();
        vector<vector<bool>> checked;

        for(int i = 0; i < m; i++) {
            vector<bool> temp(n, false);
            checked.push_back(temp);
        }

        queue<pair<int, int>> q;

        q.push(make_pair(sr, sc));
        int c = image[sr][sc];

        while(!q.empty()) {
            pair<int, int> next = q.front();
            q.pop();
            int nr = next.first;
            int nc = next.second;

            // Paint the cell
            image[nr][nc] = color;
            // Mark it as painted
            checked[nr][nc] = true;
        }
    }
};
```

The third step requires some more thinking. Let's say we wanted to add the cell right below `image[nr][nc]` to the queue. This cell is `image[nr+1][nc]`.
1. To check whether this cell is valid, we need to see if `nr+1` doesn't cross `m`, which is the height of our image.
2. To check whether the cell has been painted, `checked[nr+1][nc]` tells us this.
3. Determining the color of this cell is simply `image[nr+1][nc]`. If this value is equal to `c`, then we can go ahead and paint it.

This is how that single if statement would look like:

```cpp
if(nr + 1 < m // Step 1
    && !checked[nr+1][nc] // Step 2
    && image[nr+1][nc] == c) { // Step 3
        // Add it to the queue
        q.push(make_pair(nr+1, nc));
    }
```

And now, we do this for the up, down, left and right directions.

```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        int m = image.size();
        int n = image[0].size();
        vector<vector<bool>> checked;

        for(int i = 0; i < m; i++) {
            vector<bool> temp(n, false);
            checked.push_back(temp);
        }

        queue<pair<int, int>> q;

        q.push(make_pair(sr, sc));
        int c = image[sr][sc];

        while(!q.empty()) {
            pair<int, int> next = q.front();
            q.pop();
            int nr = next.first;
            int nc = next.second;

            image[nr][nc] = color;
            checked[nr][nc] = true;

            if(nr - 1 >= 0 && !checked[nr-1][nc] && image[nr-1][nc] == c) q.push(make_pair(nr-1, nc));
            if(nc - 1 >= 0 && !checked[nr][nc-1] && image[nr][nc-1] == c) q.push(make_pair(nr, nc-1));
            if(nr + 1 < m && !checked[nr+1][nc] && image[nr+1][nc] == c) q.push(make_pair(nr+1, nc));
            if(nc + 1 < n && !checked[nr][nc+1] && image[nr][nc+1] == c) q.push(make_pair(nr, nc+1));
        }

        return image;
    }
};
```

And we're done. We simply output the resultant image after the while loop ends.