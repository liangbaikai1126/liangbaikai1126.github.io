**思路**：利用vector建树，用数组统计每一层的结点数量，通过DFS来遍历所有结点即可
```cpp
#include<cstdio>
#include<vector>
using namespace std;

const int maxn = 100;
vector<int> children[maxn];
int levelTable[maxn];
int N, M;

void DFS(int idx, int level) {
    levelTable[level]++;
    int child_num = children[idx].size();
    if(child_num == 0) return;
    for(int i = 0; i < child_num; i++) 
        DFS(children[idx][i], level + 1);
}

int main() {
    scanf("%d %d", &N, &M);
    int num, child, father;
    for(int i = 0; i < M; i++) {
        scanf("%d %d", &father, &num);
        for(int j = 0; j < num; j++) {
            scanf("%d", &child);
            children[father].push_back(child);
        }
    }
    //根结点为1，level起始为1
    DFS(1, 1);
    int maxLevel = 0;
    for(int i = 1; i <= N; i++) 
        if(levelTable[i] > levelTable[maxLevel])
            maxLevel = i;
    printf("%d %d", levelTable[maxLevel], maxLevel);
    return 0;
}
```