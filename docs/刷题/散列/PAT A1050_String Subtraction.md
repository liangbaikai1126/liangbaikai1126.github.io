**思路**：将 s2 的每个字符统计到哈希表中，再逐个遍历 s1 中的字符。若 s1 中字符在哈希表中未出现过则输出。注意可见 ASCII 码范围为 32 - 126，故只需开辟大小为 128 的 bool 数组作为哈希表即可。

```cpp
#include<iostream>
#include<string>
using namespace std;

string s1, s2;
bool hashTable[128];

int main() {
    getline(cin, s1);
    getline(cin, s2);
    for(int i = 0; i < s2.size(); i++) {
        hashTable[s2[i]] = true;
    }
    for(int i = 0; i < s1.size(); i++) {
        if(!hashTable[s1[i]])
            cout << s1[i];
    }
    return 0;
}
```