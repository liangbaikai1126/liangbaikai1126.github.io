**思路**：直接进行建树然后与输入序列进行对比，注意镜像序列即对换左右遍历顺序
```cpp
#include<cstdio>
#include<vector>
using namespace std;

typedef struct Node Node;

struct Node {
    int data;
    Node* left, *right;
};

void insert(Node* &root, int data) {
    if(root == NULL) {
        root = new Node;
        root->data = data;
        root->left = NULL;
        root->right = NULL;
        return;
    }
    if(data < root->data) insert(root->left, data);
    else insert(root->right, data);
}

void preOrder(Node* root, vector<int>& pre) {
    if(root == NULL) return;
    pre.push_back(root->data);
    preOrder(root->left, pre);
    preOrder(root->right, pre);
}

void mPreOrder(Node* root, vector<int>& mPre) {
    if(root == NULL) return;
    mPre.push_back(root->data);
    mPreOrder(root->right, mPre);
    mPreOrder(root->left, mPre);
}

void postOrder(Node* root, vector<int>& post) {
    if(root == NULL) return;
    postOrder(root->left, post);
    postOrder(root->right, post);
    post.push_back(root->data);
}

void mPostOrder(Node* root, vector<int>& mPost) {
    if(root == NULL) return;
    mPostOrder(root->right, mPost);
    mPostOrder(root->left, mPost);
    mPost.push_back(root->data);
}


int main() {
    int n;
    scanf("%d", &n);
    int data;
    Node* root = NULL;
    vector<int> origin;
    for(int i = 0; i < n; i++) {
        scanf("%d", &data);
        insert(root, data);
        origin.push_back(data);
    }
    vector<int> pre, mPre, post, mPost;
    preOrder(root, pre);
    mPreOrder(root, mPre);
    if(origin == pre) {
        printf("YES\n");
        postOrder(root, post);
        for(int i = 0; i < post.size(); i++) {
            printf("%d", post[i]);
            if(i < post.size() - 1)
                printf(" ");
            else printf("\n");
        }
    } else if(origin == mPre) {
        printf("YES\n");
        mPostOrder(root, mPost);
        for(int i = 0; i < mPost.size(); i++) {
            printf("%d", mPost[i]);
            if(i < mPost.size() - 1)
                printf(" ");
            else printf("\n");
        }
    } else {
        printf("NO\n");
    }
    return 0;
}
```