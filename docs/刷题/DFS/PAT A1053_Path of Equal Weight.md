**思路**：利用深度优先搜索对路径进行搜索，注意判断是否到达叶结点以及剪枝。注意，题目中最后一个测试点可能出现子结点值乱序。即算法笔记中的参考答案进行的排序排序只能保证每个结点的直接子结点保持有序，但是无法保证键值相同的子结点的子结点，即孙结点有序。
比如输出序列 10 9 8 7 和 10 9 7 8，故需要把所有的答案序列进行再次排序，注意<functional>头文件包含了greater函数模板。
```cpp
#include<cstdio>
#include<vector>
#include<algorithm>
#include<functional>
using namespace std;

const int maxn = 105;

typedef struct Node Node;
struct Node {
    int data;
    vector<int> children;
};

Node node[maxn];

bool cmp(int a, int b) {
    return node[a].data > node[b].data;
}

int n, m, target;

int path[maxn];
vector<vector<int>> ans;

void DFS(int idx, int num, int sum) {
    if(sum > target) return;
    if(sum == target) {
        if(node[idx].children.size() != 0) return;
        vector<int> res;
        for(int i = 0; i < num; i++) {
            res.push_back(node[path[i]].data);
        }
        ans.push_back(res);
        return;
    }
    for(int i = 0; i < node[idx].children.size(); i++) {
        int child = node[idx].children[i];
        path[num] = child;
        DFS(child, num + 1, sum + node[child].data);
    }
}

int main() {
    scanf("%d %d %d", &n, &m, &target);
    for(int i = 0; i < n; i++) {
        scanf("%d", &node[i].data);
    }
    int idx, size, child;
    for(int i = 0; i < m; i++) {
        scanf("%d %d", &idx, &size);
        for(int j = 0; j < size; j++) {
            scanf("%d", &child);
            node[idx].children.push_back(child);
        }
        sort(node[idx].children.begin(), node[idx].children.end(), cmp);
    }
    path[0] = 0;
    DFS(0, 1, node[0].data);
    sort(ans.begin(), ans.end(), greater<vector<int>>());
    for(auto &res : ans) {
        for(int i = 0; i < res.size(); i++) {
            printf("%d", res[i]);
            if(i < res.size() - 1)
                printf(" ");
            else printf("\n");
        }
    }
    return 0;
}
```
