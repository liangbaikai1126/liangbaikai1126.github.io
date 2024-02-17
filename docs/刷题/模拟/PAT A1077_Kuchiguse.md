**思路**：将输入的字符串进行反转，将问题转化为求最长公共前缀子串。注意使用 getline 函数之前，使用 getchar 函数来接收换行符，否则输入会少一行。

```cpp
#include<iostream>
#include<string>
#include<algorithm>
#include<vector>
using namespace std;

vector<string> res;

int main() {
    int N, minLen = 300, ans = 0;
    cin >> N;
    getchar();
    string temp;
    for(int i = 0; i < N; i++) {
        getline(cin, temp);
        reverse(temp.begin(), temp.end());
        res.push_back(temp);
        if(temp.size() < minLen) minLen = temp.size();
    }
    for(int i = 0; i < minLen; i++) {
        char ch = res[0][i];
        bool isSame = true;
        for(int j = 1; j < N; j++) {
            if(ch != res[j][i]) {
                isSame = false;
                break;
            }
        }
        if(isSame) ans++;
        else break;
    }
    if(ans)
        for(int i = ans - 1; i >= 0; i--)
            cout << res[0][i];
    else cout << "nai";
    return 0;
}
```