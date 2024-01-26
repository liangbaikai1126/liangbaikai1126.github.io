**思路**：可采用Dijkstra或Dijkstra + DFS的做法。使用Dijkstra时注意优先处理最短路径，其次是幸福值，最后是平均幸福值。打印路径时可以通过一维pre数组递归得到。

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<map>
#include<string>
using namespace std;
const int MAXN = 205;
const int INF = 1E9;

int G[MAXN][MAXN], weight[MAXN], d[MAXN];
int happy[MAXN], pt[MAXN], pre[MAXN], num[MAXN];
bool visit[MAXN];
map<int, string> Int2Str;
map<string, int> Str2Int;
int N, K;

void Dijkstra(int s) {
    fill(d, d + MAXN, INF);
    d[s] = 0;
    happy[s] = weight[s];
    num[s] = 1;
    for(int i = 0; i < N; i++) {
        int u = -1, MIN = INF;
        for(int j = 0; j < N; j++) 
            if(!visit[j] && d[j] < MIN) {
                u = j;
                MIN = d[j];
            }
        if(u == -1) return;
        visit[u] = true;
        for(int v = 0; v < N; v++) 
            if(!visit[v] && d[u] + G[u][v] < d[v]) {
                d[v] = d[u] + G[u][v];
                happy[v] = happy[u] + weight[v];
                num[v] = num[u];
                pt[v] = pt[u] + 1;
                pre[v] = u;
            } else if(!visit[v] && d[u] + G[u][v] == d[v]) {
                num[v] += num[u];
                if(happy[u] + weight[v] > happy[v]) {
                    happy[v] = happy[u] + weight[v];
                    pt[v] = pt[u] + 1;
                    pre[v] = u;
                } else if(happy[u] + weight[v] == happy[v]) {
                    double uAvg = 1.0 * (happy[u] + weight[v]) / (pt[u] + 1);
                    double vAvg = 1.0 * happy[v] / pt[v];
                    if(uAvg > vAvg) {
                        pt[v] = pt[u] + 1;
                        pre[v] = u;
                    }
                }
            }
    }
}

void printPath(int v) {
    if(v == 0) {
        cout << Int2Str[v];
        return;
    }
    printPath(pre[v]);
    cout << "->" << Int2Str[v];
}

int main() {
    string city1, city2;
    cin >> N >> K >> city1;
    Str2Int[city1] = 0;
    Int2Str[0] = city1;
    for(int i = 1; i <= N - 1; i++) {
        cin >> city1 >> weight[i];
        Int2Str[i] = city1;
        Str2Int[city1] = i;
    }
    int w, id1, id2;
    fill(G[0], G[0] + MAXN * MAXN, INF);
    for(int i = 0; i < K; i++) {
        cin >> city1 >> city2 >> w;
        id1 = Str2Int[city1];
        id2 = Str2Int[city2];
        G[id1][id2] = G[id2][id1] = w;
    }
    Dijkstra(0);
    int id_rom = Str2Int["ROM"];
    printf("%d %d %d %d\n", num[id_rom], d[id_rom], happy[id_rom], happy[id_rom] / pt[id_rom]);
    printPath(id_rom);
    return 0;
}
```