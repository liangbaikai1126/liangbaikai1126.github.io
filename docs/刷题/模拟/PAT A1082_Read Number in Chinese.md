**思路**：注意以四个为一节，共有个节，万节，亿节。注意除了最后个位，每一节的末尾输出万或者亿，此外要注意如何处理中间的零。属于比较绕的模拟题。

```cpp
#include<iostream>
#include<vector>
#include<string>
using namespace std;

vector<string> num = {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};
vector<string> wei = {"Shi", "Bai", "Qian", "Wan", "Yi"};

int main() {
    string s;
    cin >> s;
    int left = 0, right = s.size() - 1;
    if(s[0] == '-') {
        cout << "Fu";
        left++;
    }
    while(left + 4 <= right) right -= 4;
    while(left < s.size()) {
        bool flag = false;
        bool isPrint = false;
        while(left <= right) {
            if(left > 0 && s[left] == '0') {
                flag = true;
            } else {
                if(flag) {
                    cout << " ling";
                    flag = false;
                }
                if(left > 0) cout << " ";
                cout << num[s[left] - '0'];
                // 该节中至少输出一位才可能输出万或亿
                isPrint = true; 
                // 某节中除了个位，都要输出十百千
                if(left != right) cout << " " << wei[right - left - 1];
            }
            left++;
        }
        // 只要不是个位，就输出万或者亿
        if(isPrint && right != s.size() - 1) 
            cout << " " << wei[(s.size() - 1 - right) / 4 + 2];
        right += 4;
    }
    return 0;
}
```