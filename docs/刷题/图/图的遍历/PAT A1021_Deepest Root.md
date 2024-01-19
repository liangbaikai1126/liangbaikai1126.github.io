**思路**：首先利用并查集确定是否能够构成一棵树，即整个图只有一个father。由于图连通且有n个结点，边数为n-1，可知一定能构成一棵树。选取任意结点进行DFS得到最大高度的端点，一定为候选的Deepest Root结点，称作集合A。再次从这些集合A的某一个结点出发，得到最大高度的端点集合称作集合B。可证（详见算法笔记）A与B的并集（注意去重）即为所求的Deepest Root结点集合。
```cpp
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;

const int maxn =10005;

vector<int> G[maxn];
int father[maxn];
int isRoot[maxn];
int n, maxHeight = 0;
vector<int> ans, res;

void init() {
    for(int i = 1; i <= n; i++)
        father[i] = i;
}

int findFather(int x) {
    int temp = x;
    while(x != father[x]) 
        x = father[x];
    int a;
    while(temp != father[temp]) {
        a = temp;
        temp = father[temp];
        father[a] = x;
    }
    return x;
}

void Union(int a, int b) {
    int fa = findFather(a);
    int fb = findFather(b);
    if(fa != fb) father[fa] = fb;
}

int blockNum() {
    int block = 0;
    for(int i = 1; i <= n; i++) 
        if(findFather(i) == i)
            block++;
    return block;
}

void DFS(int idx, int height, int pre) {
    if(height > maxHeight) {
        maxHeight = height;
        res.clear();
        res.push_back(idx);
    } else if(height == maxHeight)
        res.push_back(idx);
    for(int i = 0; i < G[idx].size(); i++) {
        if(G[idx][i] == pre) continue;
        DFS(G[idx][i], height + 1, idx);
    }
}

int main() {
    scanf("%d", &n);
    init();
    int v1, v2;
    for(int i = 0; i < n - 1; i++) {
        scanf("%d %d", &v1, &v2);
        G[v1].push_back(v2);
        G[v2].push_back(v1);
        Union(v1, v2);
    }
    int block = blockNum();
    if(block == 1) {
        DFS(1, 1, -1);
        ans = res;
        DFS(ans[0], 1, -1);
        for(int i = 0; i < res.size(); i++)
            ans.push_back(res[i]);
        sort(ans.begin(), ans.end());
        printf("%d\n", ans[0]);
        for(int i = 1; i < ans.size(); i++)
            if(ans[i] != ans[i - 1])
                printf("%d\n", ans[i]);
    } else {
        printf("Error: %d components\n", block);
    }
    return 0;
}
```