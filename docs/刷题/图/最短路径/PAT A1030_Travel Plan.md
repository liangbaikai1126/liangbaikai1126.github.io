**思路**：根据Dijkstra算法求出最短路径，用pre保存路径上每个结点的前驱。再使用DFS从终点进行倒序遍历，记录下最小的cost即为答案路径。
```cpp
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

const int MAXN = 505;
const int INF =1e9;

int N, M, S, ed, minCost = INF;
int G[MAXN][MAXN], cost[MAXN][MAXN], d[MAXN];
bool visit[MAXN];
vector<int> pre[MAXN], temp, ans;

void Dijkstra(int s) {
    fill(d, d + MAXN, INF);
    d[s] = 0;
    for(int i = 0; i < N - 1; i++) {
        int u = -1, MIN = INF;
        for(int j = 0; j < N; j++)
            if(!visit[j] && d[j] < MIN) {
                u = j;
                MIN = d[j];
            }
        if(u == -1) return;
        visit[u] = true;
        for(int v = 0; v < N; v++) 
            if(!visit[v] && G[u][v] < INF) {
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

void DFS(int v) {
    if(v == S) {
        temp.push_back(v);
        int nowCost = 0;
        for(int i = temp.size() - 1; i > 0; i--) {
            int id = temp[i], id_next = temp[i - 1];
            nowCost += cost[id][id_next];
        }
        if(nowCost < minCost) {
            minCost = nowCost;
            ans = temp;
        }
        temp.pop_back();
        return;
    }
    temp.push_back(v);
    for(int i = 0; i < pre[v].size(); i++) 
        DFS(pre[v][i]);
    temp.pop_back();
}

int main() {
    cin >> N >> M >> S >> ed;
    int u, v, dist, c;
    fill(G[0], G[0] + MAXN * MAXN, INF);
    for(int i = 0; i < M; i++) {
        cin >> u >> v >> dist >> c;
        G[u][v] = G[v][u] = dist;
        cost[u][v] = cost[v][u] = c;
    }
    Dijkstra(S);
    DFS(ed);
    for(int i = ans.size() - 1; i >= 0; i--) 
        printf("%d ", ans[i]);
    printf("%d %d\n", d[ed], minCost);
    return 0;
}
```