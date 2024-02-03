**思路**：

先将喜欢的长度为 M 的颜色序列映射到 0 ~ M-1，之后直接将不喜欢的颜色序列删去，重新整理得到映射后存留的颜色序列a。此时存留的序列一定都是喜欢的颜色。再按照LIS的模板，dp[i]代表a[i]结尾的最长序列，状态转移方程为dp[i] = max{dp[j]} + 1, 0 < j < i，且满足a[j] <= a[i]即不递减。

```cpp
//LIS
#include<cstdio>
#include<algorithm>
using namespace std;

const int MAXC = 210;
const int MAXN = 10010;

int hashTable[MAXC], a[MAXN], dp[MAXN];

int main() {
    int numColor, M;
    scanf("%d%d", &numColor, &M);
    fill(hashTable, hashTable + MAXC, -1);
    int color;
    for(int i = 0; i < M; i++) {
        scanf("%d", &color);
        hashTable[color] = i;
    }
    int totalColor, num = 0;
    scanf("%d", &totalColor);
    for(int i = 0; i < totalColor; i++) {
        scanf("%d", &color);
        if(hashTable[color] >= 0) 
            a[num++] = hashTable[color];
    }
    int ans = -1;
    for(int i = 0; i < num; i++) {
        dp[i] = 1;
        for(int j = 0; j < i; j++) 
            if(a[j] <= a[i] && dp[i] < dp[j] + 1)
                dp[i] = dp[j] + 1;
        ans = max(ans, dp[i]);
    }
    printf("%d", ans);
    return 0;
}

//LCS
#include<cstdio>
#include<algorithm>
using namespace std;
const int MAXC = 210;
const int MAXN = 10005;

int A[MAXC], B[MAXN], dp[MAXC][MAXN];

int main() {
    int N, M;
    scanf("%d%d", &N, &M);
    for(int i = 1; i <= M; i++)
        scanf("%d", &A[i]);
    int L;
    scanf("%d", &L);
    for(int i = 1; i <= L; i++)
        scanf("%d", &B[i]);
    for(int i = 1; i <= M; i++) {
        for(int j = 1; j <= L; j++) {
            dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            if(A[i] == B[j]) dp[i][j] += 1;
        }
    }
    printf("%d", dp[M][L]);
    return 0;
}
```