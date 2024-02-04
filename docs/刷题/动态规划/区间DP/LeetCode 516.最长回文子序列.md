**思路**：回文子序列和LCS的做法比较相似，s[i] = s[j] 时，状态转移方程为 dp[i][j] = dp[i + 1][j - 1] + 2。
否则为 dp[i][j] = max{dp[i + 1][j], dp[i][j - 1]} 即不选 s[i] 或者不选 s[j]。

dp[i][j] 代表从 s[i] 到 s[j] 的回文子序列最大长度，可知 i 的转移顺序从 n - 1 到 0，j 的转移顺序从 i + 1 到 n - 1。

本题可以进行空间优化，代码如下。

```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for(int i = n - 1; i >= 0; i--) {
            dp[i][i] = 1;
            for(int j = i + 1; j < n; j++) {
                if(s[i] == s[j])
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                else
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
        return dp[0][n - 1];
    }
};

//空间优化
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        vector<int> dp(n, 0);
        for(int i = n - 1; i >= 0; i--) {
            dp[i] = 1;
            int pre = 0;
            for(int j = i + 1; j < n; j++) {
                int temp = dp[j];
                if(s[i] == s[j])
                    dp[j] = pre + 2;
                else
                    dp[j] = max(dp[j], dp[j - 1]);
                pre = temp;
            }
        }
        return dp[n - 1];
    }
};
```