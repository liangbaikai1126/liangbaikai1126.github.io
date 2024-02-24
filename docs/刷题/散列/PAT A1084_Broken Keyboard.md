**思路**：用遍历的方法来判断 s1 中的字符是否出现在 s2 中，若未出现，根据 hashTable 是否记录其曾经输出过。若为 true 则曾经输出过，若为 false 则未输出过。

```cpp
#include<iostream>
#include<string>
using namespace std;

string s1, s2;

bool hashTable[128];

int main() {
    cin >> s1 >> s2;
    for(int i = 0; i < s1.size(); i++) {
        int j;
        char c1, c2;
        for(j = 0; j < s2.size(); j++) {
            c1 = s1[i], c2 = s2[j];
            if(c1 >= 'a' && c1 <= 'z') c1 -= 32;
            if(c2 >= 'a' && c2 <= 'z') c2 -= 32;
            if(c1 == c2) break;
        }
        if(j == s2.size() && hashTable[c1] == false) {
            cout << c1;
            hashTable[c1] = true;
        }
    }
    return 0;
}
```