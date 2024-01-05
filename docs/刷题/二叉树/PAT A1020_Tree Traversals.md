**思路**：利用后序和中序进行二叉树的重建，之后用BFS进行层序遍历打印
```cpp
#include<cstdio>
#include<queue>
using namespace std;

const int maxn = 35;

typedef struct Node {
    int data;
    struct Node* left;
    struct Node* right;
} Node;

int inOrder[maxn], postOrder[maxn];

Node* reCreate(int postL, int postR, int inL, int inR) {
    if(postL > postR) return NULL;
    Node* root = new Node;
    root->data = postOrder[postR];
    int i;
    for(i = inL; i <= inR; i++) 
        if(inOrder[i] == postOrder[postR]) break;
    int numL = i - inL;
    root->left = reCreate(postL, postL + numL - 1, inL, i - 1);
    root->right = reCreate(postL + numL, postR - 1, i + 1, inR);
    return root;
}

int n;
int num = 0;
void BFS(Node* root) {
    queue<Node*> q;
    q.push(root);
    while(!q.empty()) {
        Node* temp = q.front();
        q.pop();
        printf("%d", temp->data);
        num++;
        if(num < n) printf(" ");
        if(temp->left != NULL) q.push(temp->left);
        if(temp->right != NULL) q.push(temp->right);
    }
}

int main() {
    scanf("%d", &n);
    for(int i = 0; i < n; i++) 
        scanf("%d", &postOrder[i]);
    for(int i = 0; i < n; i++)
        scanf("%d", &inOrder[i]);
    Node* root = reCreate(0, n - 1, 0, n - 1);
    BFS(root);
    printf("\n");
    return 0;
}
```