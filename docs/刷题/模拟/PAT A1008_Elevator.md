**思路**：直接按顺序模拟电梯运行，注意最后一次也需要停靠 5 秒。
```cpp
#include<iostream>
using namespace std;

int main() {
    int n, now = 0, next, ans = 0;
    cin >> n;
    for(int i = 0; i < n; i++) {
        cin >> next;
        if(next > now) ans += (next - now) * 6;
        else ans += (now - next) * 4;
        now = next;
        ans += 5;
    }
    cout << ans;
    return 0;
}
``` 