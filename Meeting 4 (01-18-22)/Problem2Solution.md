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



##Solution explanation is incomplete










Solution:

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