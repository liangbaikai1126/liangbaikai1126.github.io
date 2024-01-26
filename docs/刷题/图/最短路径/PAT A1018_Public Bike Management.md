**思路**：先使用Dijkstra算法求出最短路径，使用pre记录最短路径中每个结点的前驱。再使用DFS从目标结点开始倒序遍历到PBMC，求出具有最小Need同时具有最小remain的路径。注意，倒序遍历到PBMC之后才开始计算need和remain，而且当出现remain不足当前站点所需自行车数量时，一定需要从PBMC带来自行车，也就是need单调增加，不能从后面的结点中得到的remain再补贴前面的结点。

```cpp
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
const int maxn = 505;
const int INF = 1e9;

int Cmax, N, Sp, M;
int minRemain = INF, minNeed = INF;
int G[maxn][maxn], weight[maxn], d[maxn];
bool visit[maxn];
vector<int> pre[maxn], temp, ans;

void Dijkstra(int s) {
    fill(d, d + maxn, INF);
    d[s] = 0;
    for(int i = 0; i < N; i++) {
        int u = -1, MIN = INF;
        for(int j = 0; j <= N; j++) 
            if(!visit[j] && d[j] < MIN) {
                u = j;
                MIN = d[j];
            }
        if(u == -1) return;
        visit[u] = true;
        for(int v = 0; v <= N; v++) 
            if(!visit[v] && G[u][v] != INF) {
                if(d[u] + G[u][v] < d[v]) {
                    d[v] = d[u] + G[u][v];
                    pre[v].clear();
                    pre[v].push_back(u);
                } else if(d[u] + G[u][v] == d[v]) {
                    pre[v].push_back(u);
                }
            }
    }
}

void DFS(int s) {
    if(s == 0) {
        temp.push_back(s);
        int need = 0, remain = 0;
        for(int i = temp.size() - 1; i >= 0; i--) {
            int u = temp[i];
            if(weight[u] > 0) remain += weight[u];
            else {
                if(remain > abs(weight[u]))
                    remain -= abs(weight[u]);
                else {
                    need += abs(weight[u]) - remain;
                    remain = 0;
                }
            }
        }
        if(need < minNeed) {
            minNeed = need;
            minRemain = remain;
            ans = temp;
        } else if(need == minNeed && remain < minRemain) {
            minRemain = remain;
            ans = temp;
        } 
        temp.pop_back();
        return;
    }
    temp.push_back(s);
    for(int i = 0; i < pre[s].size(); i++)
        DFS(pre[s][i]);
    temp.pop_back();
}

int main() {
    cin >> Cmax >> N >> Sp >> M;
    for(int i = 1; i <= N; i++) {
        cin >> weight[i];
        weight[i] -= Cmax / 2;
    }
    int u, v, w;
    fill(G[0], G[0] + maxn * maxn, INF);
    for(int i = 0; i < M; i++) {
        cin >> u >> v >> w;
        G[u][v] = G[v][u] = w;
    }
    Dijkstra(0);
    DFS(Sp);
    printf("%d ", minNeed);
    for(int i = ans.size() - 1; i >= 0; i--) {
        printf("%d", ans[i]);
        if(i > 0) printf("->");
    }
    printf(" %d\n", minRemain);
    return 0;
}
```