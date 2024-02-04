**思路**：本题的状态转移方程为 dp[i][j] = dp[i + 1][j - 1] && s[i] = s[j]，注意子串长度为2时特殊处理。

本题可以进行空间优化，代码如下。
```cpp
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;

const int MAXN = 1005;

bool dp[MAXN][MAXN];

int main() {
    string s;
    getline(cin, s);
    int n = s.size();
    int ans = 1;
    for(int i = n - 1; i >= 0; i--) {
        dp[i][i] = true;
        for(int j = i + 1; j < n; j++) {
            dp[i][j] = (s[i] == s[j]) && (j - i < 2 || dp[i + 1][j - 1]);
            if(dp[i][j] && j - i + 1 > ans) ans = j - i + 1; 
        }
    }
    cout << ans;
    return 0;
}

// 空间优化
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;

const int MAXN = 1005;

bool dp[MAXN];

int main() {
    string s;
    getline(cin, s);
    int n = s.size();
    int ans = 1;
    for(int i = n - 1; i >= 0; i--) {
        bool pre = false;
        dp[i] = true;
        for(int j = i + 1; j < n; j++) {
            bool temp = dp[j];
            dp[j] = (s[i] == s[j]) && (j - i < 2 || pre);
            if(dp[j] && j - i + 1 > ans) ans = j - i + 1; 
            pre = temp;
        }
    }
    cout << ans;
    return 0;
}
```