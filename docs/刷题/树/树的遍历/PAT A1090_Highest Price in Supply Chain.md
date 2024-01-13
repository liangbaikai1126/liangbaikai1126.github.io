**思路**：根据输入建立vector为结构的树，再通过DFS求得最大高度，即可解决
```cpp
#include<cstdio>
#include<cmath>
#include<vector>
using namespace std;

const int maxn = 100005;

int n, maxDepth, num, father, root;
double p, r;
vector<int> children[maxn];

void DFS(int idx, int depth) {
    int children_num = children[idx].size();
    if(children_num == 0) {
        if(depth > maxDepth) {
            maxDepth = depth;
            num = 1;
        } else if(depth == maxDepth) {
            num++;
        }
        return;
    }
    for(int i = 0; i < children_num; i++)
        DFS(children[idx][i], depth + 1);
}

int main() {
    scanf("%d %lf %lf", &n, &p, &r);
    for(int i = 0; i < n; i++) {
        scanf("%d", &father);
        if(father != -1) {
            children[father].push_back(i);
        } else {
            root = i;
        }
    }
    DFS(root, 0);
    r /= 100;
    printf("%.2f %d", p * pow(1 + r, maxDepth), num);
    return 0;
}
```