**思路**：通过下标处理将对应珠子颜色处理成哈希表中对应数字，遍历 s1 填充哈希表。再遍历 s2 减去哈希表中对应值，若哈希表中出现负数，则更新 miss。miss 大于 0，则不满足，否则直接输出 s1.size() - s2.size() 作为多余的珠子数量。

```cpp
#include<iostream>
using namespace std;

const int MAXN = 1005;

int hashTable[80], miss = 0;

string s1, s2;

int change(char ch) {
    if(ch >= '0' && ch <= '9') return ch - '0';
    else if(ch >= 'a' && ch <= 'z') return ch - 'a' + 10;
    else if(ch >= 'A' && ch <= 'Z') return ch - 'A' + 36;
    return -1;
}

int main() {
    cin >> s1 >> s2;
    for(int i = 0; i < s1.size(); i++) {
        int index = change(s1[i]);
        hashTable[index]++;
    }
    for(int i = 0; i < s2.size(); i++) {
        int index = change(s2[i]);
        hashTable[index]--;
        if(hashTable[index] < 0) miss++;
    }
    if(miss > 0) cout << "No " << miss;
    else cout << "Yes " << s1.size() - s2.size();
    return 0;
}
```