**思路**：直接用数字统计所有出现的数字的次数，按输入次序遍历哈希表即可。

```cpp
#include<iostream>
using namespace std;

const int MAXN = 100005;
const int MAXNUM = 10005;

int hashTable[MAXNUM], a[MAXN], N;

int main() {
    cin >> N;
    for(int i = 0; i < N; i++) {
        cin >> a[i];
        hashTable[a[i]]++;
    }
    int ans = -1;
    for(int i = 0; i < N; i++) {
        if(hashTable[a[i]] == 1) {
            ans = a[i];
            break;
        }
    }
    if(ans == -1) {
        cout << "None";
    } else {
        cout << ans;
    }
    return 0;
}
```