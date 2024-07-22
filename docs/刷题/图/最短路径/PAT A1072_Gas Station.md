**思路**：注意首先加油站必须满足到达所有房屋的距离不超过Ds，在此基础上，要使最近的距离最大。

当出现多个最近距离最大的解的时候，选择平均距离最小的解，优先选择索引更小的位置。使用M次Dijkstra算法将M个加油站位置全部遍历一遍可得到答案。

```cpp
#include<iostream>
#include<algorithm>
#include<vector>
#include<cstdio>
using namespace std;

const int MAXN = 1015;
const int INF = 1e9;

int N, M, K, Ds;
int G[MAXN][MAXN], d[MAXN];
bool visit[MAXN];
int ansGas = -1;
double ansDist = -1, ansAvg = INF;

int transform(string s) {
    if(s[0] != 'G') return stoi(s);
    return stoi(s.substr(1)) + N;
}

void Dijkstra(int s) {
    fill(d, d + MAXN, INF);
    fill(visit, visit + MAXN, false);
    d[s] = 0;
    for(int i = 0; i < N + M; i++) {
        int u = -1, MIN = INF;
        for(int j = 1; j <= N + M; j++)
            if(!visit[j] && d[j] < MIN) {
                u = j;
                MIN = d[j];
            }
        if(u == -1) return;
        visit[u] = true;
        for(int v = 1; v <= N + M; v++)
            if(!visit[v] && d[u] + G[u][v] < d[v]) 
                d[v] = d[u] + G[u][v];
    }
}

int main() {
    cin >> N >> M >> K >> Ds;
    string p1, p2;
    int id1, id2, dist;
    fill(G[0], G[0] + MAXN * MAXN, INF);
    for(int i = 0; i < K; i++) {
        cin >> p1 >> p2 >> dist;
        id1 = transform(p1);
        id2 = transform(p2);
        G[id1][id2] = G[id2][id1] = dist;
    }
    double avg, minDist;
    for(int i = N + 1; i <= N + M; i++) {
        Dijkstra(i);
        minDist = INF, avg = 0;
        for(int j = 1; j <= N; j++) {
            if(d[j] > Ds) {
                minDist = -1;
                break;
            }
            if(d[j] < minDist) minDist = d[j];
            avg += d[j];
        }    
        avg /= N;
        if(minDist == -1) continue;
        if(minDist > ansDist) {
            ansDist = minDist;
            ansGas = i;
            ansAvg = avg;
        } else if(minDist == ansDist && avg < ansAvg) {
            ansGas = i;
            ansAvg = avg;
        }
    }
    if(ansGas == -1) cout << "No Solution" << endl;
    else {
        cout << "G" << ansGas - N << endl;
        printf("%.1f %.1f\n", ansDist, ansAvg);
    }
    return 0;
}
```