# Problem H - [Platform Placing](https://open.kattis.com/problems/platformplacing) (Solution)

```cpp
/*
Authors: Kyle and Amin
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

const double two = 2.0;
int n, s, k;

void doIt(vector<int>& A, vector<int>& lens) {
	
	for (int i = 0; i < n; ++i) {
		
		int l = s;
		int r = k;
		int curr = s;
		
		//binary search for max width of current block (answer is stored in 'curr')
		while (l <= r) {
			int mid = (l + r) / 2;
			bool left = true;
			bool right = true;
			
			if (i > 0) {
				//can check left
				double ll = A[i];
				ll -= ((double) mid) / two;
				
				double rr = A[i - 1];
				rr += ((double) lens[i - 1]) / two;
				
				if (ll >= rr) {
					
				} else left = false;
			}
			if (i < n - 1) {
				//can check right
				double rr = A[i];
				rr += ((double) mid) / two;
				
				double ll = A[i + 1];
				ll -= ((double) lens[i + 1]) / two;
				
				if (ll >= rr) {
					
				} else right = false;
			}
			
			if (left && right) {
				curr = max(curr, mid);
				l = mid + 1;
			} else {
				r = mid - 1;
			}
		}
		
		//update length of current block to maximum width
		lens[i] = curr;
	}
}

void solve() {
	cin >> n >> s >> k;
	vector<int> A(n);
	for (int i = 0; i < n; ++i) {
		cin >> A[i];
	}
	sort(all(A));
	
	bool ok = true;
	double r = A[0];
	r += s/2.0;
	//check if answer is possible
	for (int i = 1; i < n && ok; ++i) {
		if (r > A[i] - ((double)s/two)) ok = false;
		r = max(r, (double) A[i] + ((double) s / two));
	}
	//if not possible, return -1
	if (!ok) {
		cout << -1 << '\n';
		return;
	}
	vector<int> lens(n, s);
	
	//calculate the max lengths
	doIt(A, lens);
	
	long long ans = 0;
	for (int i = 0; i < n; ++i) {
		ans += lens[i];
	}
	
	cout << ans << '\n';
}

int main() {
  ios_base::sync_with_stdio(0); cin.tie(0);
  solve();
  return 0;
}
```