# Problem A - [Beast Bullies](https://open.kattis.com/problems/beastbullies) (Solution)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<long long> animals;
    for(int i = 0; i < n; i++) {
        long long x;
        cin >> x;
        animals.push_back(x);
    }
    sort(animals.begin(), animals.end());

    int i = animals.size() - 2;
    int current_chunk = 0;
    long long last = animals[i+1];
    long long curr = 0; 

    while(i >= 0) {
        curr += animals[i];
        i--;
        if(curr >= last) {
            current_chunk = 0;
            last += curr;
            curr = 0;
        } else {
            current_chunk++;
        }
    }
    cout << animals.size() - current_chunk<< "\n";
}
```