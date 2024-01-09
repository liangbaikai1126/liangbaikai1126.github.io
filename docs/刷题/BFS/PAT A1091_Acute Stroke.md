**思路**：本题为三维BFS，本质上和二维BFS相同，不过从上下左右4种方向拓展为上下左右前后6种方向。一个连通空间即为一个核，达到阈值T的核可以算进总体积中。注意BFS中每次调用BFS相当于走过了一个连通区域，需要设置visit数组防止重复访问已经访问的坐标。

```cpp
#include<cstdio>
#include<queue>
using namespace std;

int M, N, L, T, ans;

typedef struct {
    int x, y, z;
} Node;

int dx[6] = {0, 0, 0, 0, 1, -1};
int dy[6] = {0, 0, 1, -1, 0, 0};
int dz[6] = {1, -1, 0, 0, 0, 0};

int graph[1290][130][61];
bool visit[1290][130][61];

bool judge(int x, int y, int z) {
    if(x < 0 || x >= M || y < 0 || y >= N || z < 0 || z >= L) {
        return false;
    }
    if(graph[x][y][z] == 0 || visit[x][y][z] == true) {
        return false;
    }
    return true;
}

int BFS(int x, int y, int z) {
    queue<Node> q;
    Node temp;
    temp.x = x;
    temp.y = y;
    temp.z = z;
    q.push(temp);
    visit[x][y][z] = true;
    int res = 0;
    while(!q.empty()) {
        Node node = q.front();
        q.pop();
        res++;
        for(int i = 0; i < 6; i++) {
            int nx = node.x + dx[i];
            int ny = node.y + dy[i];
            int nz = node.z + dz[i];
            if(judge(nx, ny, nz)) {
                temp.x = nx;
                temp.y = ny;
                temp.z = nz;
                q.push(temp);
                visit[nx][ny][nz] = true;
            }
        }
    }
    if(res < T) return 0;
    return res;
}

int main() {
    scanf("%d %d %d %d", &M, &N, &L, &T);
    for(int k = 0; k < L; k++) 
        for(int i = 0; i < M; i++) 
            for(int j = 0; j < N; j++) 
                scanf("%d", &graph[i][j][k]);
    for(int z = 0; z < L; z++) 
        for(int x = 0; x < M; x++)
            for(int y = 0; y < N; y++)
                if(graph[x][y][z] == 1 && visit[x][y][z] == false)
                    ans += BFS(x, y, z);
    printf("%d", ans);
    return 0;
}
```