**思路**：根结点默认为0，用静态数组建树，输入的键值进行递增排序。按照中序遍历的顺序对每个结点填入键值，最后BFS求得层序遍历答案。
```cpp
#include<cstdio>
#include<algorithm>
#include<queue>
using namespace std;

const int maxn = 105;

typedef struct {
    int val, left, right;
} Node;

Node node[maxn];
int n, key[maxn], index;

void inOrder(int root) {
    if(root == -1) return;
    inOrder(node[root].left);
    node[root].val = key[index++];
    inOrder(node[root].right);
}

void BFS(int root) {
    queue<int> q;
    q.push(root);
    while(!q.empty()) {
        int temp = q.front();
        q.pop();
        printf("%d", node[temp].val);
        index++;
        if(index < n) printf(" ");
        if(node[temp].left != -1) q.push(node[temp].left);
        if(node[temp].right != -1) q.push(node[temp].right);
    }
}

int main() {
    scanf("%d", &n);
    for(int i = 0; i < n; i++) 
        scanf("%d %d", &node[i].left, &node[i].right);
    for(int i = 0; i < n; i++)
        scanf("%d", &key[i]);
    sort(key, key + n);
    inOrder(0);
    index = 0;
    BFS(0);
    return 0;
}
```