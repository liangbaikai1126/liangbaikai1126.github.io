**思路**：先将多项式 A 输入并封装到 vector 中，再开设一个大小为 2000 的数组 ans 来记录答案，ans[i] 代表 coef，i 代表 exp。
```cpp
#include<iostream>
#include<cstdio>
#include<vector>
using namespace std;

typedef struct {
    int exp;
    double coef;
} Poly;

double ans[2005];
vector<Poly> p;

int main() {
    int k, exp;
    double coef;
    cin >> k;
    Poly temp;
    for(int i = 0; i < k; i++) {
        cin >> exp >> coef;
        temp.exp = exp;
        temp.coef = coef;
        p.push_back(temp);
    }
    cin >> k;
    for(int i = 0; i < k; i++) {
        cin >> exp >> coef;
        for(int j = 0; j < p.size(); j++) {
            ans[exp + p[j].exp] += coef * p[j].coef;
        }
    }
    int num = 0;
    for(int i = 0; i <= 2000; i++)
        if(ans[i] != 0.0) num++;
    cout << num;
    for(int i = 2000; i >= 0; i--)
        if(ans[i] != 0.0)
            printf(" %d %.1f", i, ans[i]);
    return 0;
}
```