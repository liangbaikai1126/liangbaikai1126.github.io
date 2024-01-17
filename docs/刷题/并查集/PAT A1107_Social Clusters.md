**思路**：将最先喜欢某个爱好的人作为根节点，使用并查集进行解题
```cpp
#include<cstdio>
#include<algorithm>
#include<functional>
using namespace std;

const int maxn = 1005;

int hobby[maxn];
int father[maxn];
int res[maxn];

int n;

int findFather(int x) {
    int temp = x;
    while(father[x] != x) {
        x = father[x];
    }
    int a;
    while(father[temp] != temp) {
        a = temp;
        temp = father[temp];
        father[a] = x;
    }
    return x;
}

void Union(int a, int b) {
    int fa = findFather(a);
    int fb = findFather(b);
    if(fa != fb) {
        father[fa] = fb;
    }
}

int main() {
    scanf("%d", &n);
    for(int i = 1; i <= n; i++)
        father[i] = i;
    int num, temp;
    for(int i = 1; i <= n; i++) {
        scanf("%d:", &num);
        for(int j = 0; j < num; j++) {
            scanf("%d", &temp);
            if(hobby[temp] == 0) {
                hobby[temp] = i;
            }
            Union(i, findFather(hobby[temp]));
        }
    }
    int ans = 0;
    for(int i = 1; i <= n; i++) 
        res[findFather(i)]++;
    for(int i = 1; i <= n; i++) 
        if(res[i] != 0) ans++;
    printf("%d\n", ans);
    sort(res + 1, res + 1 + n, greater<int>());
    for(int i = 1; i <= ans; i++) {
        printf("%d", res[i]);
        if(i < ans) printf(" ");
    }
    return 0;
}
```