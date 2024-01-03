**思路**：注意处理前导零以及小数点比如000100.00123和000.123两种情况，以及无小数点的000123456等
```cpp
#include<iostream>
#include<string>
using namespace std;

int n;

string normalize(string s, int &e) {
    while(s.length() > 0 && s[0] == '0')
        s.erase(s.begin());
    if(s[0] == '.') {
        s.erase(s.begin());
        while(s.length() > 0 && s[0] == '0') {
            s.erase(s.begin());
            e--;
        }
    } 
    else {
        int k = 0; 
        while(k < s.length() && s[k] != '.') {
            k++;
            e++;
        }
        if(k < s.length()) {
            s.erase(s.begin() + k);
        }
    }
    if(s.length() == 0) {
        e = 0;
    }
    int num = 0, idx = 0;
    string res;
    while(num < n) {
        if(idx < s.length()) {
            res += s[idx];
            ++idx;
        } else {
            res += "0";
        }
        num++;
    }
    return res;
}

int main() {
    string s1, s2;
    cin >> n >> s1 >> s2;
    int e1 = 0, e2 = 0;
    s1 = normalize(s1, e1);
    s2 = normalize(s2, e2);
    if(s1 == s2 && e1 == e2) {
        cout << "YES 0." << s1 << "*10^" << e1 << endl;
    } else {
        cout << "NO 0." << s1 << "*10^" << e1 << " 0." << s2 << "*10^" << e2 << endl;        
    }
    return 0;
}
```