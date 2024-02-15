**思路**：直接进行暴力模拟，注意全程使用字符串，使用浮点数或者长整型会溢出。

```cpp
#include<iostream>
using namespace std;

int main() {
    string s;
    cin >> s;
    if(s[0] == '-') cout << "-";
    char first = s[1];
    int posE = 3;
    while(s[posE] != 'E') posE++;

    int exp = 0;
    for(int i = posE + 2; i < s.size(); i++)
        exp = exp * 10 + s[i] - '0';
    bool isNeg = (s[posE + 1] == '-');
    s = s.substr(3, posE - 3);
    
    if(exp == 0) {
        cout << first << "." << s;
    } else if(!isNeg) {
        if(exp >= s.size()) {
            cout << first << s;
            for(int i = 0; i < exp - s.size(); i++)
                cout << "0";
        } else {
            cout << first << s.substr(0, exp) << "." << s.substr(exp);
        }
    } else {
        cout << "0.";
        exp--;
        for(int i = 0; i < exp; i++)
            cout << "0";
        cout << first << s;
    }
    return 0;
}
```