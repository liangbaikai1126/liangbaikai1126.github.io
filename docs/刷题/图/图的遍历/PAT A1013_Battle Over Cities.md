**思路**：本题可以用DFS和并查集两种做法。

**DFS**：对图进行DFS遍历，得到若干个连通分量，需要补充的道路数量即为连通分量-1

**并查集**：对被删除之外的点进行Union，最后查询集合个数，等同于连通分量数

```cpp
//DFS
#include<cstdio>
#include<vector>
#include<cstring>
using namespace std;

const int maxn = 1005;

vector<int> G[maxn];
bool visit[maxn];
int N, M, K;
int delVertex;

void DFS(int v) {
    if(v == delVertex) return;
    visit[v] = true;
    for(int i = 0; i < G[v].size(); i++) 
        if(!visit[G[v][i]]) DFS(G[v][i]);
}

int main() {
    scanf("%d %d %d", &N, &M, &K);
    int v1, v2;
    for(int i = 0; i < M; i++) {
        scanf("%d %d", &v1, &v2);
        G[v1].push_back(v2);
        G[v2].push_back(v1);
    }
    int block;
    for(int q = 0; q < K; q++) {
        scanf("%d", &delVertex);
        memset(visit, false, sizeof(visit));
        block = 0;
        for(int i = 1; i <= N; i++) {
            if(i != delVertex && !visit[i]) {
                DFS(i);
                block++;
            }
        }
        printf("%d\n", block - 1);
    }
    return 0;
}
```

```cpp
//并查集
#include<cstdio>
#include<vector>
using namespace std;

const int maxn = 1005;

vector<int> G[maxn];
bool visit[maxn];
int father[maxn];
int N, M, K;
int delVertex;

int findFather(int x) {
    int temp = x;
    while(x != father[x])
        x = father[x];
    int a;
    while(temp != father[temp]) {
        a = temp;
        temp = father[temp];
        father[a] = x;
    }
    return x;
}

void Union(int a, int b) {
    int fa = findFather(a);
    int fb = findFather(b);
    if(fa != fb) father[fa] = fb;
}

int main() {
    scanf("%d %d %d", &N, &M, &K);
    int v1, v2;
    for(int i = 0; i < M; i++) {
        scanf("%d %d", &v1, &v2);
        G[v1].push_back(v2);
        G[v2].push_back(v1);
    }
    int block;
    for(int q = 0; q < K; q++) {
        scanf("%d", &delVertex);
        for(int i = 1; i <= N; i++) {
            father[i] = i;
            visit[i] = false;
        }
        for(int i = 1; i <= N; i++) {
            for(int j = 0; j < G[i].size(); j++) {
                v1 = i, v2 = G[i][j];
                if(v1 == delVertex || v2 == delVertex)
                    continue;
                Union(v1, v2);
            }
        }
        block = 0;
        for(int i = 1; i <= N; i++) {
            if(i == delVertex) continue;
            int f = findFather(i);
            if(!visit[f]) {
                block++;
                visit[f] = true;
            }
        }
        printf("%d\n", block - 1);
    }
    return 0;
}
```