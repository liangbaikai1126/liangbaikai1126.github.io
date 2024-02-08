**思路**：整体思路类似于归并排序，将输入数据封装成结构后按指数大小排序，采用队列的做法。注意系数相加后若为 0，该项不存在，不能加入答案。
```cpp
#include<iostream>
#include<cstdio>
#include<queue>
using namespace std;

typedef struct {
    int exp;
    double coef;
} Term;

int main() {
    queue<Term> A, B;
    int k, e;
    double c;
    Term temp;
    cin >> k;
    for(int i = 0; i < k; i++) {
        cin >> e >> c;
        temp.exp = e;
        temp.coef = c;
        A.push(temp);
    }
    cin >> k;
    for(int i = 0; i < k; i++) {
        cin >> e >> c;
        temp.exp = e;
        temp.coef = c;
        B.push(temp);
    }
    vector<Term> ans;
    Term a, b;
    while(!A.empty() && !B.empty()) {
        a = A.front();
        b = B.front();
        if(a.exp == b.exp) {
            temp.exp = a.exp;
            temp.coef = a.coef + b.coef;
            if(temp.coef != 0) ans.push_back(temp);
            A.pop();
            B.pop();
        } else if(a.exp > b.exp) {
            ans.push_back(a);
            A.pop();
        } else {
            ans.push_back(b);
            B.pop();
        }
    }
    while(!A.empty()) {
        a = A.front();
        ans.push_back(a);
        A.pop();
    }
    while(!B.empty()) {
        b = B.front();
        ans.push_back(b);
        B.pop();
    }
    int lens = ans.size();
    cout << lens;
    for(int i = 0; i < lens; i++) {
        printf(" %d %.1f", ans[i].exp, ans[i].coef);
    }
    return 0;
}
```