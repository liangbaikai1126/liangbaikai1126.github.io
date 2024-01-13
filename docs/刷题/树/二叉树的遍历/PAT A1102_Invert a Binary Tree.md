**思路**：设置标志数组notRoot，将作为左右子结点都排除为根节点，可找到根节点。接着根据后序遍历的顺序来Invert整颗树，利用BFS进行层序遍历。
```cpp
#include<cstdio>
#include<queue>
#include<algorithm>
#include<iostream>
using namespace std;

const int maxn = 15;

typedef struct {
    int left, right;
} Node;

Node node[maxn];
bool notRoot[maxn];
int n, num;

int ToNumber(char ch) {
    if(ch == '-') return -1;
    notRoot[ch - '0'] = true;
    return ch - '0';
}

int findRoot() {
    for(int i = 0; i < n; i++)
        if(!notRoot[i]) return i;
    return -1;
}

void invertTree(int root) {
    if(root == -1) return;
    invertTree(node[root].left);
    invertTree(node[root].right);
    swap(node[root].left, node[root].right);
}

void levelOrder(int root) {
    queue<int> q;
    q.push(root);
    while(!q.empty()) {
        int temp = q.front();
        q.pop();
        printf("%d", temp);
        num++;
        if(num < n) printf(" ");
        if(node[temp].left != -1) q.push(node[temp].left);
        if(node[temp].right != -1) q.push(node[temp].right);
    }
    num = 0;
    printf("\n");
}

void inOrder(int root) {
    if(root == -1) return;
    inOrder(node[root].left);
    printf("%d", root);
    num++;
    if(num < n) printf(" ");
    inOrder(node[root].right);
}

int main() {
    cin >> n;
    char left, right;
    for(int i = 0; i < n; i++) {
        cin >> left >> right;
        node[i].left = ToNumber(left);
        node[i].right = ToNumber(right);
    }
    int root = findRoot();
    invertTree(root);
    levelOrder(root);
    inOrder(root);
    return 0;
}
```