**思路**：直接进行题目需要的模拟，但是注意判断字母的范围，第一次为 A ~ G，第二次为 A ~ N，第三次则需考虑全部小写字母和大写字母。

```cpp
#include<iostream>
#include<algorithm>
#include<string>
#include<cstdio>
using namespace std;

string s1, s2, s3, s4;

string weekday[7] = {"MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN"};

bool isNeedCapital(char ch, int op) {
    if(op == 0) return ch >= 'A' && ch <= 'G';
    else if(op == 1) return ch >= 'A' && ch <= 'N';
    else if(op == 2) return (ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z');
    return ch >= '0' && ch <= '9';
}

int main() {
    cin >> s1 >> s2 >> s3 >> s4;
    int len1 = s1.size(), len2 = s2.size();
    int len3 = s3.size(), len4 = s4.size();
    int i;
    for(i = 0; i < len1 && i < len2; i++) {
        if(isNeedCapital(s1[i], 0) && s1[i] == s2[i]) {
            cout << weekday[s1[i] - 'A'] << " ";
            break;
        }
    }
    for(i = i + 1; i < len1 && i < len2; i++) {
        if(s1[i] == s2[i]) {
            if(isNeedCapital(s1[i], 1)) {
                printf("%02d:", s1[i] - 'A' + 10);
                break;
            } else if(isNeedCapital(s1[i], 3)) {
                printf("%02d:", s1[i] - '0');
                break;
            }
        } 
    }
    for(i = 0; i < len3 && i < len4; i++) {
        if(isNeedCapital(s3[i], 2) && s3[i] == s4[i]) {
            printf("%02d", i);
            break;
        }
    }
    return 0;
}
```