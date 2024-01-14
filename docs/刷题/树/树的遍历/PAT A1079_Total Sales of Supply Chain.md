**思路**：本题与A1090相似，vector建树然后使用DFS，不同之处在于叶结点多了点权。
```cpp
#include<cstdio>
#include<vector>
#include<cmath>
using namespace std;

const int maxn = 100005;

typedef struct {
    double val;
    vector<int> children;
} Node;

Node node[maxn];

int n;
double p, r, res;

void DFS(int idx, int depth) {
    int child_num = node[idx].children.size();
    if(child_num == 0) {
        res += node[idx].val * pow(1 + r, depth);
        return;
    }
    for(int i = 0; i < child_num; i++) 
        DFS(node[idx].children[i], depth + 1);
}

int main() {
    scanf("%d %lf %lf", &n, &p, &r);
    r /= 100;
    int num, child;
    for(int i = 0; i < n; i++) {
        scanf("%d", &num);
        if(num) {
            for(int j = 0; j < num; j++) {
                scanf("%d", &child);
                node[i].children.push_back(child);
            }
        } else {
            scanf("%lf", &node[i].val);
        }
    }
    // 注意根结点已经说明是0
    DFS(0, 0);
    printf("%.1f", p * res);
    return 0;
}
```