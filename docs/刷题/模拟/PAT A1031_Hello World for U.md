**思路**：使用二维数组进行模拟，注意 n1 + n2 + n3 = N + 2，其中 2 的含义是 n1 和 n3 与 n2 在左下角和右下角存在重合。

```cpp
#include<iostream>
using namespace std;

int main() {
    char ans[40][40];
    string s;
    cin >> s;
    int k = (s.size() + 2) / 3;
    int n2 = s.size() + 2 - 2 * k;
    for(int i = 0; i < 40; i++)
        for(int j = 0; j < 40; j++)
            ans[i][j] = ' ';
    int pos = 0;
    for(int i = 0; i < k; i++)
        ans[i][0] = s[pos++];
    for(int i = 1; i < n2; i++)
        ans[k-1][i] = s[pos++];
    for(int i = k - 2; i >= 0; i--)
        ans[i][n2-1] = s[pos++];
    for(int i = 0; i < k; i++) {
        for(int j = 0; j < n2; j++)
            cout << ans[i][j];
        cout << endl;
    }
    return 0;
}
```