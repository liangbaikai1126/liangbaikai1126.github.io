**思路**：本题为图结构的遍历题，题意中给出转发次数上限，即按层来遍历邻接结点，单步步长使用BFS即可解决。每次相邻的结点入队时，层数加一，即转发层数的限制。

```cpp
#include<iostream>
#include<vector>
#include<cstring>
#include<queue>
using namespace std;

typedef struct {
    int id, layer;
} Node;

const int maxn = 1005;
vector<Node> G[maxn];
bool visit[maxn];
int N, K, L;

int BFS(int idQuery, int Layer) {
    queue<Node> q;
    Node s;
    s.id = idQuery;
    s.layer = 0;
    q.push(s);
    visit[s.id] = true;
    int res = 0;
    while(!q.empty()) {
        Node t = q.front();
        q.pop();
        int id = t.id;
        for(int i = 0; i < G[id].size(); i++) {
            Node next = G[id][i];
            next.layer = t.layer + 1;
            if(!visit[next.id] && next.layer <= L) {
                q.push(next);
                visit[next.id] = true;
                res++;
            }
        }
    }
    return res;
}

int main() {
    cin >> N >> L;
    Node user;
    int numFollow, idFollow;
    for(int i = 1; i <= N; i++) {
        user.id = i;
        cin >> numFollow;
        for(int j = 0; j < numFollow; j++) {
            cin >> idFollow;
            G[idFollow].push_back(user);
        }
    }
    cin >> K;
    int idQuery;
    for(int k = 0; k < K; k++) {
        cin >> idQuery;
        memset(visit, false, sizeof(bool) * (N + 1));
        int numForward = BFS(idQuery, L);
        cout << numForward << endl;
    }
    return 0;
}
```