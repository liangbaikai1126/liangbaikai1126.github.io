**思路**：一共有三种操作，插入，删除和替换。状态转移方程为，若 s[i] = t[j]，dp[i][j] = dp[i - 1][j - 1]。d
若 s[i] != t[j]，dp[i][j] = min{dp[i][j - 1]（插入）, dp[i - 1][j]（删除）, dp[i - 1][j - 1]（替换）} + 1。

**空间优化**：本题依赖的状态转移可用滚动数组进行优化成两个一维数组，两个一维数组的状态转移仅依赖于左，上，左上，三个方向，其中左上会被左覆盖。
因此可以设置临时变量来保存左上的状态，优化成一维数组。
```cpp
class Solution {
public:
    int min(int a, int b, int c) {
        if(a < b) return a > c ? c : a;
        return b > c ? c : b;
    }

    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for(int j = 0; j < n; j++) {
            dp[0][j + 1] = j + 1;
        }
        for(int i = 0; i < m; i++) {
            dp[i + 1][0] = i + 1;
            for(int j = 0; j < n; j++) {
                if(word1[i] == word2[j]) {
                    dp[i + 1][j + 1] = dp[i][j];
                } else {
                    dp[i + 1][j + 1] = min(dp[i + 1][j], dp[i][j + 1], dp[i][j]) + 1;
                }
            }
        }
        return dp[m][n];
    }
};

//空间优化
class Solution {
public:
    int min(int a, int b, int c) {
        if(a < b) return a > c ? c : a;
        return b > c ? c : b;
    }

    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<int> dp(n + 1, 0);
        for(int j = 0; j < n; j++) {
            dp[j + 1] = j + 1;
        }
        int pre, temp;
        for(int i = 0; i < m; i++) {
            pre = dp[0];
            dp[0] += 1;
            for(int j = 0; j < n; j++) {
                temp = dp[j + 1];
                if(word1[i] == word2[j]) {
                    dp[j + 1] = pre;
                } else {
                    dp[j + 1] = min(dp[j], temp, pre) + 1;
                }
                pre = temp;
            }
        }
        return dp[n];
    }
};
```