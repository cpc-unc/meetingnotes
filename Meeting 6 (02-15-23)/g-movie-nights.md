# Problem G - [Movie Night](https://open.kattis.com/problems/movienight) (Solution)


```cpp
/**************
MOVIE NIGHTS SOLUTION

Author: Amin
***************
*/

#include <bits/stdc++.h>
using namespace std;

#define sz(x) (int)(x).size()
#define pb push_back
#define f first
#define s second
#define lb lower_bound
#define ub upper_bound
#define all(x) begin(x), end(x)
#define ins insert

const int mxN = 1e5 + 1;
const long long mod = 1e9 + 7;
vector<int> adj[mxN];

long long cnts[mxN];
int indeg[mxN];
bool vis[mxN];

long long dfs(int u, long long curr =0) {
	vis[u] = true;
	curr = curr + (((curr*cnts[u]) % mod) + cnts[u] % mod);
	curr %= mod;
	
	for (int j : adj[u]) {
		if (vis[j]) {
			return (curr + 1) % mod;
		} else {
			return dfs(j, curr);
		}
	}
	return curr;
}

void solve() {
	int n; cin >> n;
	for (int i = 1; i<=n; ++i) {
		int u; cin >> u;
		adj[i].pb(u);
		indeg[u]++;
	}
	
	
	//start topological sort
	queue<int> q;
	
	for (int i = 1; i <= n; ++i) {
		if (indeg[i] == 0) {
			q.push(i);
		}
	}
	
	long long ans = 0;
	while (sz(q)) {
		auto u = q.front(); q.pop();
		vis[u] = true;
		cnts[u]++;
		cnts[u] %= mod;
		for (int j : adj[u]) {
			//add answer from current branch (u) to adjacent branch (j)
			cnts[j] = cnts[j] + ((((cnts[j]*cnts[u]) % mod) + cnts[u]) % mod);
			cnts[j] %= mod;
			indeg[j]--;
			if (indeg[j] == 0) q.push(j);
		}
	}
	//end topo sort
	
	//run around cycle of different connected components
	for (int i = 1; i <= n; ++i) {
		
		if (!vis[i]) {
			long long temp = dfs(i); //calculate answer for current component
      
			//add to total answer
			ans = ((ans + ((ans*temp) % mod)) % mod) + temp;
      ans %= mod;
		}
	}
	cout << ans << '\n';
}

int main() {
  ios_base::sync_with_stdio(0); cin.tie(0);
  solve();
  return 0;
}

```
