# Problem 2 Solution - [Evaluate division](https://leetcode.com/problems/evaluate-division/)
## Try to attempt this problem yourself before you read the solution!

This problem is a pure DFS algorithm with a little bit more extra logic.

The way I'm going to try to support a graph that holds all the equation information, is with an adjacency map.
If we're looking for all the nodes that `'a'` points to, we query the map with `graph["a"]` which gives us another map of all the equations that have `'a'` as a numerator. 

For example, if these are the equations in our input:
$$\frac{a}{b} = 3/2$$
$$\frac{b}{c} = 5/2$$
$$\frac{d}{e} = 5$$

Then our map looks like this.

```
graph["a"] = {"b": 3/2}
graph["b"] = {"a": 2/3, "c": 5/2}
graph["d"] = {"e": 5}
graph["e"] = {"d": 1/5}
```

Basically, any query `graph[x][y]` gives us the value of `x/y`. If you notice the graph defines `"a"/"b"` as $3/2$, which automatically means `"b"/"a"` is $2/3$, which is also information contained within the graph.

To store this information we use the data type `unordered_map<string, unordered_map<string, double>>`. Since both are unordered maps (order of variables added doesn't matter), the retrieval of any equation value `graph[x][y]` is $O(1)$, because unordered maps are based on a hashmap.

Here's two things I want to implement in my code immediately:
1. Let's also keep track of a set of `string`s so that we know which variables are known. This is an `unordered_set` called `vars` which also takes $O(1)$ retrieval.
2. If an equation $a/b$ has the value 0, then I automatically know that `'a'` is 0. So I want to store all the `zeros` in another unordered set. Finding out whether a variable `x` is equal to zero (with `zeros.count("x")`) takes $O(1)$ time.

```cpp
class Solution {
public:
    unordered_map<string, unordered_map<string, double>> graph;
    unordered_set<string> vars;

    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_set<string> zeros;

        // This is our answer. It's empty for now
        vector<double> answer;

        // This loop iterates through all equations so that we can store it in our graph.
        for(int i = 0; i < equations.size(); i++) {
            string a = equations[i][0];
            string b = equations[i][1];

            // We insert these variables into our set.
            // Since it's a set, inserting a value again does nothing.
            vars.insert(a);
            vars.insert(b);

            double v = values[i];

            if(v == 0) {
                // If it's 0, we don't put it in the graph. I'll explain why later.
                zeros.insert(a);
            } else {
                // We need to store values for a/b and b/a in our graph
                graph[a][b] = v;
                graph[b][a] = 1/v;
            }
        }
    }
};
```

Now we try to answer our queries.

For any query `queries[i]`, We're trying to find the value of `queries[i][0] / queries[i][1]`.
Here's a couple of corner cases to look out for.
1. If `queries[i][0]` or `queries[i][1]` is a variable that *has never been seen before*, the answer to this query is automatically -1, since there's no way of figuring it out.
2. If `queries[i][0]` = 0, then `0 / queries[i][1]` = 0 no matter what `queries[i][1]` is.
3. If `queries[i][1]` = 0, then `queries[i][0] / 0` is undefined no matter what `queries[i][0]` is. So the query output is automatically -1.
4. If `queries[i][0]` = `queries[i][1]`, then the answer is automatically 1.

For any of these cases, we simply append the corresponding answer to our `answer` vector/array.

```cpp
class Solution {
public:
    unordered_map<string, unordered_map<string, double>> graph;
    unordered_set<string> vars;

    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_set<string> zeros;
        vector<double> answer;

        for(int i = 0; i < equations.size(); i++) {
            string a = equations[i][0];
            string b = equations[i][1];

            vars.insert(a);
            vars.insert(b);

            double v = values[i];

            if(v == 0) {
                zeros.insert(a);
            } else {
                graph[a][b] = v;
                graph[b][a] = 1/v;
            }
        }

        for(int i = 0; i < queries.size(); i++) {
            marked.clear();
            if(!vars.count(queries[i][0]) || !vars.count(queries[i][1])) {
                answer.push_back(-1);
            } else if(zeros.count(queries[i][0])) {
                answer.push_back(0);
            } else if(zeros.count(queries[i][1])) {
                answer.push_back(-1);
            } else if(queries[i][1] == queries[i][0]) {
                answer.push_back(1);
            } else {
                // ...i wish this the else block was that simple
            }
        }
    }
};
```

Now let's try to implement the bulk of the task: the DFS algorithm specific to this task.
Let's say we're trying to find the value `a / d`, and we have the values of `a / b`, `b / c`, and `c / d`.
Then, $\frac{a}{d} = \frac{a}{b} * \frac{b}{c} * \frac{c}{d}$. This can be found because `a` is connected to `b`, which is connected to `c`, which is connected to `d`. The way we say this is `a` &rarr; `b` &rarr; `c` &rarr; `d`.

What if we didn't have `b / c` but we did have `d / b`?
Then, $\frac{a}{d} = \frac{a}{b} * \frac{b}{d}$. Again, since we have `d / b`, we know what `b / d` is.

## Theorem

If we have a path of nodes $x_1$ &rarr; $x_2$ &rarr; ... &rarr; $x_n$, (assuming $x_i \neq 0$ for all i = ${1, ..., n}$), then we can find the value of $x_1 / x_n$.

## Proof

Let's say we have a path from $x_i$ to $x_{i+1}$. This means we know what $x_i / x_{i+1}$ is. Let $k_i$ = $x_i / x_{i+1}$, which is known for all $i$.
Then $\frac{x_1}{x_n} = \frac{x_1}{x_2} * \frac{x_2}{x_3} * ... * \frac{x_{n-1}}{x_n} = k_1 * k_2 * ... * k_n$


So, this problem can now simply turn into a DFS algorithm:

```cpp
void dfs(int s) {
    if (visited[s]) return;
        visited[s] = true;
        // process node s
        for (auto u: adj[s]) {
            dfs(u);
        }
}
```

I don't believe this is the most efficient solution of a DFS for this algorithm. But it's pretty explanatory.

If I call the function `dfs` with the parameters `start` and `end`, I want the value `start / end`. This function will output 2 values, `ans` (double) and `found` (boolean) as a `pair<double, bool>` in C++. If `found` is false, then we haven't been able to find `start / end` from this node. If `found` is true, then `start / end` = `ans`.
The parameter `current` is the node that it is currently processing.

An explanation of the value `v` can be given through an example. If there's a path `a` &rarr; `b` &rarr; `c` &rarr; `d` and we're currently processing the node `d`, the value `v` is going to be equal to `a/b * b/c * c/d`.

## Example of what we want to do

If we're trying to find `a / d`, and we have a path `a` &rarr; `b` &rarr; `c` &rarr; `d`. Let's say we also have a path `a` &rarr; `c` &rarr; `e`. We're gonna start with `a` by running `dfs("a", "d", "a", 1)`. (`v` = 1 because the only one in the chain so far is `a`, which means `a/a` = 1. See the definition for `v` above).

Let's say we're going down the list, and we encounter the node `b` which is currently the `current` node.

Here's what we want to do:
1. Iterate through all the nodes that `b` points to (which in this case, is just `c` as `b` &rarr; `c`).
    For each node that `b` points to (which we call `i` in the code):
    1. See if `i` is marked. If it is, don't process on `i` anymore, move onto the next node `i` that `b` is adjacent to.
        (Spoiler: `i` = `c` isn't marked.)
    2. If `i` equals `d`, then we have a complete path from `a` to `d`. Then we return the multiplied value of the fractions from `a` to `d`.
        Since `v` = `a/b`, the answer should be `a/b * b/d`, which is `v * b/d`.
        The value of `b/d` can be obtained by finding `graph["b"]["d"]`. In this case, `i` already gives us the pair `("c", b/d)` (which is kind of like a string-double tuple).
        `i.first` gives us the string `"d"` and `i.second` gives us the double value of the equation `b/d`.
        So now the new output should equal `v * i.second`, and `true` for the boolean output as we have now found the answer.
    3. If `i` doesn't equal `d` (which in this case it won't. It's going to only equal `c`).
        1. Run the DFS again on this sub-node.
            Then the "new" `v` is going to equal `v` * `b/c` = `a/b * b/c`.
            This function call will be `dfs("a", "d", "c", v * i.second)` (as `i.second` gives us the actual value of `graph[current][i]`, or in this case, `graph["b"]["c"]` which is `b/c`).
        2. If this sub-DFS returns true, then we return this value.
2. If we're outside the for loop, that means we haven't returned anything yet. This means that there is *no path* from `current` to `end`. So, we output `false` for the boolean value.

Again, I want a set called `marked` to keep track of which nodes are visited.
(Btw, I the first function's code has been removed temporarily to explain the functionality of the DFS).

```cpp
class Solution {
    unordered_set<string> marked;

    pair<double, bool> dfs(string start, string end, string current, double v) {
        marked.insert(current); // Mark the current node so we don't process it again

        // We get the map of all the variables that current points to
        unordered_map adj = graph[current];

        bool found = false;
        double ans = v;

        for(auto i : adj) {
            // i is a pair of numbers.
            // current / i.first = i.second
                // This means current and i.first are both strings, and i.second is a double.

            // Step 1.1
            if(marked.count(i.first)) continue;

            if(i.first == end) {
                // Step 1.2
                return make_pair(v * i.second, true); 
            } else {
                // Step 1.3
                auto nxt = dfs(start, end, i.first, v*i.second);
                if(nxt.second){
                    // Step 1.3.2
                    return make_pair(nxt.first, nxt.second);
                }
            }
        }

        // Step 2
        return make_pair(v, false);
    }
};
```

And now, we finally finish the else statement that was incomplete:

```cpp

// Bunch of removed code

    for(int i = 0; i < queries.size(); i++) {
        marked.clear();
        if(!vars.count(queries[i][0]) || !vars.count(queries[i][1])) {
            answer.push_back(-1);
        } else if(zeros.count(queries[i][0])) {
            answer.push_back(0);
        } else if(zeros.count(queries[i][1])) {
            answer.push_back(-1);
        } else if(queries[i][1] == queries[i][0]) {
            answer.push_back(1);
        } else {

            // Run the DFS on the starting node. The value of v, naturally, is 1.
            auto ans = dfs(queries[i][0], queries[i][1], queries[i][0], 1);

            // If the DFS returns false, then we can't find the answer of this query. Then we just push -1 as the answer.
            answer.push_back((ans.second) ? ans.first : -1);
        }
    }
```

And we put all this code together, and we're done.

```cpp
class Solution {
public:
    unordered_map<string, unordered_map<string, double>> graph;
    unordered_set<string> vars;
    unordered_set<string> marked;

    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_set<string> zeros;
        vector<double> answer;

        for(int i = 0; i < equations.size(); i++) {
            string a = equations[i][0];
            string b = equations[i][1];

            vars.insert(a);
            vars.insert(b);

            double v = values[i];

            if(v == 0) {
                zeros.insert(a);
            } else {
                graph[a][b] = v;
                graph[b][a] = 1/v;
            }
        }

        for(int i = 0; i < queries.size(); i++) {
            marked.clear();
            if(!vars.count(queries[i][0]) || !vars.count(queries[i][1])) {
                answer.push_back(-1);
            } else if(zeros.count(queries[i][0])) {
                answer.push_back(0);
            } else if(zeros.count(queries[i][1])) {
                answer.push_back(-1);
            } else if(queries[i][1] == queries[i][0]) {
                answer.push_back(1);
            } else {
                auto ans = dfs(queries[i][0], queries[i][1], queries[i][0], 1);
                answer.push_back((ans.second) ? ans.first : -1);
            }
        }
        
        return answer;
    }

    pair<double, bool> dfs(string start, string end, string current, double v) {
        marked.insert(current);

        unordered_map adj = graph[current];

        bool found = false;
        double ans = v;

        for(auto i : adj) {
            if(marked.count(i.first)) continue;

            if(i.first == end) {
                return make_pair(v * i.second, true);
            } else {
                auto nxt = dfs(start, end, i.first, v*i.second);
                if(nxt.second){
                    return make_pair(nxt.first, nxt.second);
                }
            }
        }

        return make_pair(v, false);
    }
};
```