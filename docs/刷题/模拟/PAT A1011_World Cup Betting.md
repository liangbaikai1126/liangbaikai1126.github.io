**思路**：直接进行简单的取最大值乘积模拟。

```cpp
#include<iostream>
#include<cstdio>
using namespace std;

char mp[3] = {'W', 'T', 'L'};

int main() {
    double ans = 1.0, temp, a;
    int idx;
    for(int i = 0; i < 3; i++) {
        temp = 0.0;
        for(int j = 0; j < 3; j++) {
            cin >> a;
            if(a > temp) {
                temp = a;
                idx = j;
            }
        }
        ans *= temp;
        printf("%c ", mp[idx]);
    }
    printf("%.2f", (ans * 0.65 - 1) * 2);
    return 0;
}
```