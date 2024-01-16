**思路**：平衡二叉树的模板题目，注意AVL树每次插入后都要调整高度，然后进行相应的左旋和右旋
```cpp
#include<cstdio>
#include<algorithm>
using namespace std;

typedef struct Node {
    int data, height;
    struct Node* left, * right;
} Node;

int n;

Node* createNode(int val) {
    Node* node = new Node;
    node->data = val;
    node->height = 1;
    node->left = node->right = NULL;
    return node;
}

int getHeight(Node* root) {
    if(root == NULL) return 0;
    return root->height;
}

void updateHeight(Node* root) {
    root->height = max(getHeight(root->left), getHeight(root->right)) + 1;
}

// 这里定义平衡因子数值为左子树与右子树的高度差
int getBalanceFactor(Node* root) {
    return getHeight(root->left) - getHeight(root->right);
}

void leftRotation(Node* &root) {
    Node* temp = root->right;
    root->right = temp->left;
    temp->left = root;
    updateHeight(root);
    updateHeight(temp);
    root = temp;
}

void rightRotation(Node* &root) {
    Node* temp = root->left;
    root->left = temp->right;
    temp->right = root;
    updateHeight(root);
    updateHeight(temp);
    root = temp;
}

void insert(Node* &root, int val) {
    if(root == NULL) {
        root = createNode(val);
        return;
    }
    if(root->data < val) {
        insert(root->right, val);
        updateHeight(root);
        if(getBalanceFactor(root) == -2) {
            // RR
            if(getBalanceFactor(root->right) == -1) {
                leftRotation(root);
            // RL
            } else if(getBalanceFactor(root->right) == 1) {
                rightRotation(root->right);
                leftRotation(root);
            }
        } 
    } else {
        insert(root->left, val);
        updateHeight(root);
        if(getBalanceFactor(root) == 2) {
            // LL
            if(getBalanceFactor(root->left) == 1) {
                rightRotation(root);
            // LR
            } else if(getBalanceFactor(root->left) == -1) {
                leftRotation(root->left);
                rightRotation(root);
            }
        }
    }
}

int main() {
    scanf("%d", &n);
    int val;
    Node* root = NULL;
    for(int i = 0; i < n; i++) {
        scanf("%d", &val);
        insert(root, val);
    }
    printf("%d", root->data);
    return 0;
} 
```