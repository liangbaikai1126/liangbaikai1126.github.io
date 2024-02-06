**思路**：本题为0-1背包的变形版，求0-1背包的最小字典序问题。其中 dp[i][j] 代表从 1 ~ i 号硬币中选择，在最大支付额度为 j 的条件下能支付的最大金额。由于本题中负重和价值为同一数据，由状态转移方程 dp[i][j] = max{dp[i-1][j], dp[i-1][j - coins[i]] + coins[i]} 知其最大金额不会超出最大额度。所以判断是否有解的条件为 dp[n][m] == m，其中 n 为硬币总数，m 为目标支付额度。

**最小字典序**：将硬币从大到小排序，先考虑大硬币，遍历到小硬币时可以进行拆分，即 dp[i][j] <= dp[i-1][j-coins[i]] + coins[i] 时进行的拆分。求出路径时，对于 i 号硬币，判断 dp[i][j] == dp[i-1][j-coins[i]] + coins[i] 成立即需要选择该硬币。

**空间优化**：将 dp 优化成一维数组，根据状态转移方程需要改变 v 的遍历方向，另需要设置 choose 和 flag 数组来判断，代码可见《算法笔记》。

```cpp
// 二维数组版
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

const int MAXN = 10005, MAXM = 105;
int coins[MAXN], dp[MAXN][MAXM];
int n, m;
vector<int> ans;

bool cmp(int a, int b) {
    return a > b;
}

int main() {
    cin >> n >> m;
    for(int i = 1; i <= n; i++)
        cin >> coins[i];
    sort(coins + 1, coins + n + 1, cmp);
    // dp过程
    for(int i = 1; i <= n; i++) {
        for(int v = coins[i]; v <= m; v++) {
            dp[i][v] = max(dp[i-1][v], dp[i-1][v-coins[i]] + coins[i]);
        }
    }
    // 输出路径
    if(dp[n][m] != m) cout << "No Solution";
    else {
        int j = m;
        for(int i = n; i >= 1; i--) {
            if(j >= coins[i] && dp[i][j] == dp[i - 1][j - coins[i]] + coins[i]) {
                ans.push_back(coins[i]);
                j -= coins[i];
            }
        }
        for(int i = 0; i < ans.size(); i++) {
            cout << ans[i];
            if(i < ans.size() - 1) cout << " ";
        }
    }
    return 0;
}
```