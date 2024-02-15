**思路**：简单的回文模拟判断，注意当 N = 0 时直接判断为回文数。

```cpp
#include<iostream>
#include<vector>
using namespace std;

vector<int> res;

void GetDigits(int N, int b) {
    while(N != 0) {
        res.push_back(N % b);
        N /= b;
    }
}

bool JudgePalin() {
    int lens = res.size();
    for(int i = 0; i < lens / 2; i++) {
        if(res[i] != res[lens - i - 1])
            return false;
    }
    return true;
}

int main() {
    int N, b;
    cin >> N >> b;
    if(N == 0) {
        cout << "Yes" << endl;
        cout << "0";
        return 0;
    }
    GetDigits(N, b);
    if(JudgePalin() == true) cout << "Yes" << endl;
    else cout << "No" << endl;
    for(int i = res.size() - 1; i >= 0; i--) {
        cout << res[i];
        if(i != 0) cout << " ";
    }
    return 0;
}
```