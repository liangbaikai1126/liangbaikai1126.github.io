**思路**：直接模拟栈的出栈和入栈，题目中有按时Push操作相当于先序遍历，而Pop操作相当于中序遍历。由先序+中序或后序+中序可重构二叉树，即重建树并进行后序遍历。
```cpp
#include<cstdio>
#include<stack>
#include<iostream>
#include<vector>
#include<string>
using namespace std;

stack<int> st;
int n, num;
string action;
vector<int> pre, in, post;

typedef struct Node {
    int val;
    Node *left, *right;
} Node;

void postOrder(Node *root) {
    if(root == NULL) return;
    postOrder(root->left);
    postOrder(root->right);
    post.push_back(root->val);
}

Node* reCreate(int preL, int preR, int inL, int inR) {
    if(preL > preR) return NULL;
    Node* root = new Node;
    root->val = pre[preL];
    int k;
    for(k = inL; k <= inR; k++)
        if(in[k] == pre[preL])
            break;
    int numLeft = k - inL;
    root->left = reCreate(preL + 1, preL + numLeft, inL, k - 1);
    root->right = reCreate(preL + numLeft + 1, preR, k + 1, inR);
    return root;
}

int main() {
    scanf("%d", &n);
    for(int i = 0; i < 2 * n; i++) {
        cin >> action;
        if(action == "Push") {
            cin >> num;
            st.push(num);
            pre.push_back(num);
        } else if(action == "Pop") {
            num = st.top();
            st.pop();
            in.push_back(num);
        }
    }
    Node *root = reCreate(0, n - 1, 0, n - 1);
    postOrder(root);
    for(int i = 0; i < n; i++) {
        printf("%d", post[i]);
        if(i < n - 1) printf(" ");
    }
    return 0;
}
```