**思路**：由于PAT输入数据的特殊性，本题考虑用静态链表来实现。

注意输入数据中可能有无效的结点，因此需要县遍历一次并且确定有效结点数order，并且为每个有效结点附上顺序标记。

根据顺序标记进行排序后直接对静态链表数组中前order个结点进行输出，每k个结点倒序输出，最后一组若不满足k个结点则按正常顺序输出。

**相似题目**：LeetCode 25.K个一组反转链表
```cpp
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn = 100005;
typedef struct {
    int addr, data, next;
    int order;
} Node;
Node node[maxn];
bool cmp(Node a, Node b) {
    return a.order < b.order;
}
int main() {
    int s1, n, k;
    scanf("%d %d %d", &s1, &n, &k);
    int addr, data, next;
    for(int i = 0; i < maxn; i++) 
        node[i].order = maxn;
    for(int i = 0; i < n; i++) {
        scanf("%d %d %d", &addr, &data, &next);
        node[addr].addr = addr;
        node[addr].data = data;
        node[addr].next = next;
    }
    int p = s1, order = 0;
    while(p != -1) {
        node[p].order = order++;
        p = node[p].next;
    }
    sort(node, node + maxn, cmp);
    for(int i = 0; i < order / k; i++) {
        for(int j = (i + 1) * k - 1; j > i * k; j--) 
            printf("%05d %d %05d\n", node[j].addr, node[j].data, node[j-1].addr);
        printf("%05d %d ", node[i * k].addr, node[i * k].data);
        if(i < order / k - 1)
            printf("%05d\n", node[(i + 2) * k - 1].addr);
        else {
            if(order % k == 0) 
                printf("-1\n");
            else {
                printf("%05d\n", node[(i + 1) * k].addr);
                for(int m = order / k * k; m < order; m++) {
                    printf("%05d %d ", node[m].addr, node[m].data);
                    if(m < order - 1)
                        printf("%05d\n", node[m+1].addr);
                    else printf("-1\n");
                }
            }
        }
    }
    return 0;
}
```