**思路**：利用一维dp数组，dp[i]代表以a[i]为结尾的子序列的最大值，并且使用s[i]记录子序列第一个值的下标。
状态转移方程为 dp[i] = max(dp[i-1] + a[i], a[i])
```cpp
#include<cstdio>
#include<algorithm>
using namespace std;

const int MAXN = 10005;

int dp[MAXN], a[MAXN], s[MAXN];
int K;

int main() {
    scanf("%d", &K);
    bool flag = false;
    for(int i = 0; i < K; i++) {
        scanf("%d", &a[i]);
        if(a[i] >= 0) flag = true;
    }
    if(!flag) {
        printf("0 %d %d", a[0], a[K - 1]);
        return 0;
    }
    dp[0] = a[0];
    for(int i = 1; i < K; i++) {
        if(dp[i - 1] + a[i] > a[i]) {
            dp[i] = dp[i - 1] + a[i];
            s[i] = s[i-1];
        } else {
            dp[i] = a[i];
            s[i] = i;
        }
    }
    int res = 0;
    for(int i = 1; i < K; i++)
        if(dp[i] > dp[res])
            res = i;
    printf("%d %d %d", dp[res], a[s[res]], a[res]);
    return 0;
}
```