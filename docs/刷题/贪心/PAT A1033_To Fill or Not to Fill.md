**思路**：注意根据贪心策略，将加油站根据距离的远近进行排序。由于初始油箱为空，若最初的加油站距离不为 0，则无法到达目的地。

从 0 号加油站开始遍历，遍历能到达且油价最低（除了当前油价）的加油站，设置为目的加油站。

若遍历到第一个小于当前油价的加油站 A，立刻结束遍历，因为可以在当前加油站加到刚好到 A 的油量，到达 A 之后使用的油价格更低。

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
using namespace std;

const int MAXN = 505;
const int INF = 1e9;

typedef struct {
    double price, dist;
} Station;

Station st[MAXN];
double C_max, D, D_avg;
int N;

bool cmp(Station a, Station b) {
    return a.dist < b.dist;
}

int main() {
    cin >> C_max >> D >> D_avg >> N;
    for(int i = 0; i < N; i++) {
        cin >> st[i].price >> st[i].dist;
    }
    st[N].price = 0;
    st[N].dist = D;
    sort(st, st + N, cmp);
    if(st[0].dist != 0) {
        printf("The maximum travel distance = 0.00\n");
        return 0;
    }
    double ans = 0, nowTank = 0, maxDist = C_max * D_avg;
    int nowStation = 0;
    while(nowStation < N) {
        int idx = -1;
        double minPrice = INF;
        for(int i = nowStation + 1; i <= N &&
            st[i].dist - st[nowStation].dist <= maxDist; i++) {
            if(st[i].price < minPrice) {
                minPrice = st[i].price;
                idx = i;
                // 现在不加满，加到刚好够更低价的下一站
                if(minPrice < st[nowStation].price) {
                    break;
                }
            }
        }
        if(idx == -1) break;
        double needTank = (st[idx].dist - st[nowStation].dist) / D_avg;
        if(minPrice < st[nowStation].price) {
            if(nowTank < needTank) {
                ans += (needTank - nowTank) * st[nowStation].price;
                nowTank = 0;
            } else {
                nowTank -= needTank;
            }
        } else {
            ans += (C_max - nowTank) * st[nowStation].price;
            nowTank = C_max - needTank;
        }
        nowStation = idx;
    }
    if(nowStation == N) {
        printf("%.2f\n", ans);
    } else {
        printf("The maximum travel distance = %.2f\n", st[nowStation].dist + maxDist);
    }
    return 0;
}
```