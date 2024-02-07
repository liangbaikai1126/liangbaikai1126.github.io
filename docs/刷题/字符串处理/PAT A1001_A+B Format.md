**思路**：先进行符号处理，再计算除 3 结果 cnt 和模 3 的余数 remain，根据 cnt 和 remain 的不同来处理，化简成倍数为 3 的子串问题。

```cpp
#include<iostream>
#include<string>
using namespace std;

int main() {
    int a, b, sum;
    cin >> a >> b;
    sum = a + b;
    if(sum < 0) {
        sum = -sum;
        cout << "-";
    }
    string s = to_string(sum);
    int cnt = s.size() / 3, remain = s.size() % 3;
    if(cnt == 0) {
        cout << s;
        return 0;
    }
    if(remain == 1) {
        cout << s[0] << ",";
        s = s.substr(1);
    }
    if(remain == 2) {
        cout << s[0] << s[1] << ",";
        s = s.substr(2);
    }
    cnt--;
    for(int i = 0; i < s.size(); i++) {
        if(cnt > 0 && i > 0 && i % 3 == 0) {
            cout << ",";
            cnt--;
        }
        cout << s[i];
    }
    return 0;
}
```