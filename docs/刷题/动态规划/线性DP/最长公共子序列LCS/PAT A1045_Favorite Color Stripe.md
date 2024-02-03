**思路**：本题为 LCS 模板题的变形。原本 LCS 状态转移方程中，若 A[i] == B[j]，则 dp[i][j] = dp[i - 1][j - 1] + 1（可证明）。

由于本题中可以产生重复元素匹配，此时转移方程变为 dp[i][j] = max{dp[i - 1][j], dp[i][j - 1]} + 1。

边界：dp[i][0] = dp[0][j] = 0 (0 <= i <= n, 0 <= j <= n)

```cpp
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