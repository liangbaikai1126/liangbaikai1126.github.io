**思路**：将输入的数据封装成新结构，并且处理输入的时间字符串为以秒为单位的整型，通过两次排序来确定最早和最晚的人。

```cpp
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;

typedef struct {
    string id;
    int inSec, outSec;
} Info;

vector<Info> res;

bool cmp_in(Info a, Info b) {
    return a.inSec < b.inSec;
}

bool cmp_out(Info a, Info b) {
    return a.outSec > b.outSec;
}

int strToInt(string s) {
    int h, m, sec;
    h = (s[0] - '0') * 10 + (s[1] - '0');
    m = (s[3] - '0') * 10 + (s[4] - '0');
    sec = (s[6] - '0') * 10 + (s[7] - '0');
    sec += h * 60 * 60 + m * 60;
    return sec;
}

int main() {
    int M;
    string inTime, outTime, id;
    cin >> M;
    Info temp;
    for(int i = 0; i < M; i++) {
        cin >> id >> inTime >> outTime;
        temp.id = id;
        temp.inSec = strToInt(inTime);
        temp.outSec = strToInt(outTime);
        res.push_back(temp);
    }
    sort(res.begin(), res.end(), cmp_in);
    cout << res[0].id << " ";
    sort(res.begin(), res.end(), cmp_out);
    cout << res[0].id << endl;
    return 0;
}
```