**思路**：利用Dijkstra算法，从源点出发求出到其他所有点的最短路径数量以及点权和。
```cpp
#include<iostream>
#include<algorithm>
using namespace std;

const int maxn = 505;
const int INF = 1e9;

int rescue[maxn], G[maxn][maxn];
int weight[maxn], d[maxn], num[maxn];
bool visit[maxn];
int N, M, start, ed;

void Dijkstra(int s) {
    fill(d, d + maxn, INF);
    d[s] = 0;
    weight[s] = rescue[s];
    num[s] = 1;
    // 注意这里只需要循环 N - 1 次
    // 比如 N = 2 时，只需要 2 - 1 = 1 次就可以得到最短路径
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
            if(!visit[v] && G[u][v] != INF) {
                if(d[u] + G[u][v] < d[v]) {
                    d[v] = d[u] + G[u][v];
                    weight[v] = weight[u] + rescue[v];
                    num[v] = num[u];
                } else if(d[u] + G[u][v] == d[v]) {
                    if(weight[u] + rescue[v] > weight[v]) 
                        weight[v] = weight[u] + rescue[v];
                    num[v] += num[u];
                }
            }
    }
}

int main() {
    cin >> N >> M >> start >> ed;
    for(int i = 0; i < N; i++)
        cin >> rescue[i];
    int u, v, w;
    fill(G[0], G[0] + maxn * maxn, INF);
    for(int i = 0; i < M; i++) {
        cin >> u >> v >> w;
        G[v][u] = G[u][v] = w;
    }
    Dijkstra(start);
    cout << num[ed] << " " << weight[ed] << endl;
    return 0;
}
```