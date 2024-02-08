**思路**：先把输入当成字符串，求出每位和 sum 后将 sum 转化成字符串输出单词。
```cpp
#include<iostream>
#include<string>
using namespace std;

string s;
int sum;

void numToWord(char ch) {
    if(ch == '0') cout << "zero";
    else if(ch == '1') cout << "one";
    else if(ch == '2') cout << "two";
    else if(ch == '3') cout << "three";
    else if(ch == '4') cout << "four";
    else if(ch == '5') cout << "five";
    else if(ch == '6') cout << "six";
    else if(ch == '7') cout << "seven";
    else if(ch == '8') cout << "eight";
    else if(ch == '9') cout << "nine";
} 

int main() {
    cin >> s;
    for(auto ch : s) sum += (ch - '0');
    s = to_string(sum);
    for(int i = 0; i < s.size(); i++) {
        numToWord(s[i]);
        if(i < s.size() - 1) cout << " ";
    }
    return 0;
}
```