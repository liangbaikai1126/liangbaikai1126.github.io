**思路**：简单的溢出判断题，由于 A B C 均处于 long long 的数据范围内。

如果 A + B 的真值大于 long long 上限，则 A + B 的真值一定大于 C，此时 A > 0, B > 0, A + B < 0

如果 A + B 的真值小于 long long 下限，则 A + B 的真值一定小于 C，此时 A < 0, B < 0, A + B > 0
```cpp
#include<iostream>
using namespace std;

int main() {
    int T;
    long long A, B, C, res;
    bool flag;
    cin >> T;
    for(int i = 1; i <= T; i++) {
        cin >> A >> B >> C;
        res = A + B;
        if(A > 0 && B > 0 && res < 0) flag = true;
        else if(A < 0 && B < 0 && res > 0) flag = false;
        else if(res > C) flag = true;
        else flag = false;
        cout << "Case #" << i << ": ";
        if(flag) cout << "true";
        else cout << "false";
        cout << endl;
    }
    return 0;
}
```
