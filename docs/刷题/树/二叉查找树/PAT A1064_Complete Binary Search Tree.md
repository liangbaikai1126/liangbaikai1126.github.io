**思路**：利用数组建立完全二叉树，数组按顺序遍历即为完全二叉树的层序遍历
```cpp
#include<cstdio>
#include<algorithm>
using namespace std;

const int maxn = 1005;

int CBT[maxn], val[maxn];
int n, index;

void inOrder(int root) {
    if(root > n) return;
    inOrder(2 * root);
    CBT[root] = val[index++];
    inOrder(2 * root + 1);
}

int main() {
    scanf("%d", &n);
    for(int i = 0; i < n; i++) {
        scanf("%d", &val[i]);
    }
    sort(val, val + n);
    inOrder(1);
    for(int i = 1; i <= n; i++) {
        printf("%d", CBT[i]);
        if(i < n) printf(" ");
    }
    return 0;
}
```