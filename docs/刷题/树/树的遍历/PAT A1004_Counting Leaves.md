**思路**：vector建树，DFS遍历结点，通过更新最大高度来确定有多少层。并且用数组来记录每一层的叶子结点。
```cpp
#include<cstdio>
#include<vector>
using namespace std;

const int maxn = 105;

vector<int> children[maxn];
int N, M, maxDepth;
int res[maxn];

void DFS(int idx, int depth) {
    int child_num = children[idx].size();
    if(child_num == 0) {
        res[depth]++;
        if(depth > maxDepth) {
            maxDepth = depth;
        }
        return;
    }
    for(int i = 0; i < child_num; i++) 
        DFS(children[idx][i], depth + 1);
}

int main() {
    scanf("%d %d", &N, &M);
    int father, num, child;
    for(int i = 0; i < M; i++) {
        scanf("%d %d", &father, &num);
        for(int j = 0; j < num; j++) {
            scanf("%d", &child);
            children[father].push_back(child);
        }
    }
    DFS(1, 0);
    for(int i = 0; i <= maxDepth; i++) {
        printf("%d", res[i]);
        if(i < maxDepth) printf(" ");
    }
    return 0;
}
```