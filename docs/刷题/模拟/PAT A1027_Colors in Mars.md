**思路**：简单的进制转换模拟，使用数组更加快捷。

```cpp
#include<iostream>
using namespace std;

char mp[13] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C'};
int a[3];

int main() {
    cin >> a[0] >> a[1] >> a[2];
    cout << "#";
    for(int i = 0; i < 3; i++) {
        cout << mp[a[i] / 13] << mp[a[i] % 13];
    }
    return 0;
}
```