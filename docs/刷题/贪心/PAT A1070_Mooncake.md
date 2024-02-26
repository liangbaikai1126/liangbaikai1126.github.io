**思路**：简单的贪心策略，优先卖出单价最高的月饼。

```cpp
#include<iostream>
#include<cstdio>
#include<algorithm>
using namespace std;

const int MAXN = 1005;

typedef struct {
    double store, totalPrice, price;
} Mooncake;

Mooncake mc[MAXN];

int n, totalNeed;

bool cmp(Mooncake a, Mooncake b) {
    return a.price > b.price;
}

int main() {
    cin >> n >> totalNeed;
    for(int i = 0; i < n; i++) {
        cin >> mc[i].store;
    }
    for(int i = 0; i < n; i++) {
        cin >> mc[i].totalPrice;
        mc[i].price = mc[i].totalPrice / mc[i].store;
    }
    sort(mc, mc + n, cmp);
    double ans = 0;
    for(int i = 0; i < n; i++) {
        if(mc[i].store <= totalNeed) {
            totalNeed -= mc[i].store;
            ans += mc[i].totalPrice;
        } else {
            ans += mc[i].price * totalNeed;
            break;
        }
    }
    printf("%.2f", ans);
    return 0;
}
```