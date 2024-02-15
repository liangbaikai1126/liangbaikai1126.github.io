**思路**：预处理从 1 号结点到 N 号节点的距离之和，另外使用 dist 数组，dist[i - 1] 代表 1 号结点到 i 号结点的距离。
故 dist(left, right) (left < right) 可以表示成 dist[right - 1] - dist[left - 1]。

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

const int MAXN = 100005;
int N, M;
int D[MAXN], dist[MAXN];

int main() {
    cin >> N;
    int sum = 0;
    for(int i = 1; i <= N; i++) {
        cin >> D[i];
        sum += D[i];
        dist[i] = sum;
    }
    cin >> M;
    int left, right, temp;
    for(int i = 0; i < M; i++) {
        cin >> left >> right;
        if(left > right) swap(left, right);
        temp = dist[right - 1] - dist[left - 1];
        cout << min(temp, sum - temp) << endl;
    }
    return 0;
}
```