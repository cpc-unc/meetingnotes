# Problem L - [Branch Manager](https://nasouth22d1.kattis.com/contests/nasouth22d1/problems/branchmanager)

```cpp
/*
Author: Amin
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

#define LOG 18
const int mxN = 2e5 + 1;
int n, d;
int lst; //last visited node


set<int> adj[mxN];
int parents[mxN][LOG + 1];
int st[mxN], en[mxN];
int depths[mxN];
int times = 1;

struct Fen {
	
	vector<int> tt;
	int _n; 
	void init(int nn) {
		_n = nn;
		tt.resize(_n + 3, 0);
	}
	
	int get(int i) {
		int ss = 0;
		while (i > 0) {
			ss += tt[i];
			i -= i & -i;
		}
		return ss;
	}
	
	void add(int i, int dx) {
		
		while (i <= _n) {
			tt[i] += dx;
			i += i & -i;
		}
	}
};

Fen fenny;

void build_parents() {
	for (int k = 1; k <= LOG; ++k) {
		for (int i = 1; i <= n; ++i) {
			int p = parents[i][k - 1];
			if (p == -1) continue;
			parents[i][k] = parents[p][k - 1];
		}
	}
}


void tour(int root, int depth=0) {
	st[root] = times++;
	depths[root] = depth;
	for (int j : adj[root]) {
		tour(j, depth + 1);
	}
	en[root] = times++;
}

bool is_unreachable(int u) {
	return fenny.get(st[u]) > 0;
}

void set_unreachable(int u) {
	fenny.add(st[u], 1);
	fenny.add(en[u] + 1, -1);
}

int getUp(int u, int dist) {
	for (int k = LOG; k >= 0; --k) {
		if (dist & (1 << k)) {
			u = parents[u][k];
		}
	}
	return u;
}
int lca(int a, int b) {
	if (a == b) return a;
	int dp = min(depths[a], depths[b]);
	int curr = 0;
	int ans = 1;
	int temp;
	for (int k = LOG; k >= 0; --k) {
		int nxt_curr = curr + (1 << k);
		if (nxt_curr > dp) continue;
		
		if ((temp = getUp(a, depths[a] - nxt_curr)) == getUp(b, depths[b] - nxt_curr)) {
			curr = nxt_curr;
			ans = temp;
		}
	
	}
	return ans;
}

int setup(int u) {
	if (sz(adj[u]) == 0) return u;
	return setup(*begin(adj[u]));
}

int visit(int source, int dest) {
	int x = lca(source, dest);
	if (x == dest) return dest;
	vector<int> toRem;
	int nxt;
	for (int j : adj[x]) {
		if (lca(j, dest) == j) {
			nxt = j;
			break;
		}
		toRem.pb(j);
	}
	for (int j : toRem) {
		set_unreachable(j);
		adj[x].erase(j);
	}
	return dest;
}

void solve() {
	cin >> n >> d;
	for (int i = 0; i < n-1; ++i) {
		int a, b; cin >> a >> b;
		adj[a].ins(b);
		parents[b][0] = a;
	}
	parents[1][0] = -1;
	build_parents();
	tour(1);
	fenny.init(times + 2);
	lst = setup(1);
	
	int ans = 0;
	
	for (int i = 0; i < d; ++i) {
		int nxt; cin >> nxt;
		if (is_unreachable(nxt)) break;
		lst = visit(lst, nxt);
		ans++;
	}
	
	cout << ans << '\n';
	
	
}

int main() {
  ios_base::sync_with_stdio(0); cin.tie(0);
  solve();
  return 0;
}
```