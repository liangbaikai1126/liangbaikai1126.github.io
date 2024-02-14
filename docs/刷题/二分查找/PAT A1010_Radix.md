**思路**：本题使用二分法来查找答案进制数字，先将已经确定进制的输入 N1 转化为十进制，再将待确定进制的输入 N2 二分查找转化为十进制。需要注意使用 long long 类型来避免溢出问题。最重要的是确定上界和下界，下界为 N2 的各位中最大值 + 1，上界为 N1 的十进制值和下界二者中取最大值。注意两个输入数据相同时，输入给出的 radix 即为目标进制。 

```cpp
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;

string N1, N2;
int tag, radix;

int chToInt(char ch) {
    return isalpha(ch) ? (ch - 'a' + 10) : (ch - '0');
}

int calLowBound(string s) {
    int res = 0;
    for(int i = 0; i < s.size(); i++) {
        res = max(res, chToInt(s[i]));
    }
    return res + 1;
}

long long calDecVal(string s, long long a) {
    if(a == 10) return (long long)atoi(s.c_str());
    long long res = 0, k = 1;
    for(int i = s.size() - 1; i >= 0; i--) {
        res += k * chToInt(s[i]);
        k *= a;
    }
    return res;
}

long long calRadix(string s, long long x, long long L, long long R) {
    long long mid, temp;
    // 闭区间二分
    while(L <= R) {
        mid = (L + R) / 2;
        temp = calDecVal(s, mid);
        if(temp == x) return mid;
        else if(temp > x || temp < 0) R = mid - 1;
        else L = mid + 1;
    }
    return -1;
}



int main() {
    cin >> N1 >> N2 >> tag >> radix;
    if(N1 == N2) {
        cout << radix;
        return 0;
    }
    if(tag == 2) swap(N1, N2);
    long long N1_DecVal = calDecVal(N1, radix);
    long long L = calLowBound(N2);
    long long R = max(N1_DecVal, L);
    long long ans = calRadix(N2, N1_DecVal, L, R);
    if(ans != -1) cout << ans;
    else cout << "Impossible";
    return 0;
}
```