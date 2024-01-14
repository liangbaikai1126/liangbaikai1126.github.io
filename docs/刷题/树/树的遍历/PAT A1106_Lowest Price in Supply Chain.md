**思路**：根据输入建立vector为结构的树，再通过DFS求得最小高度，同时记录最小高度结点的数量，即可解决
```cpp
#include<cstdio>
#include<vector>
#include<cmath>
using namespace std;

const int maxn = 100005;

vector<int> children[maxn];
double p, r;
int n, minDepth = maxn, cnt;

void DFS(int idx, int depth) {
    int child_num = children[idx].size();
    if(child_num == 0) {
        if(depth < minDepth) {
            minDepth = depth;
            cnt = 1;
        }
        else if(depth == minDepth) cnt++;
        return;
    }
    for(int i = 0; i < child_num; i++)
        DFS(children[idx][i], depth + 1);
}

int main() {
    scanf("%d %lf %lf", &n, &p, &r);
    r /= 100;
    int num, child;
    for(int i = 0; i < n; i++) {
        scanf("%d", &num);
        for(int j = 0; j < num; j++) {
            scanf("%d", &child);
            children[i].push_back(child);
        }
    }
    DFS(0, 0);
    printf("%.4f %d", p * pow(1 + r, minDepth), cnt);
    return 0;
}
```