**思路**：遍历给出的静态链表，通过设置标记数组记录每个键值是否已经出现过。

每个结点赋予一个order排序值，初始时所有结点的order初始化为2*maxn，未出现过的键值结点赋值valid从0开始，最终值为有效结点的数量。

出现过要被删除的结点键值按照removed + maxn进行赋值，removed从0开始，最终值为被删除的结点数量。调用sort按照order进行排序。
```cpp
#include<cstdio>
#include<algorithm>
using namespace std;

const int maxn = 100005;
const int table_size = 10005;
typedef struct {
    int addr, key, next, order;
} Node;

Node node[maxn];
bool keyExist[table_size];

int abs(int x) {
    if(x > 0) return x;
    return -x;
}

bool cmp(Node a, Node b) {
    return a.order < b.order;
}

int main() {
    int s1, n;
    scanf("%d %d", &s1, &n);
    int addr, key, next;
    for(int i = 0; i < n; i++) {
        scanf("%d %d %d", &addr, &key, &next);
        node[addr].addr = addr;
        node[addr].key = key;
        node[addr].next = next;
    }
    for(int i = 0; i < maxn; i++)
        node[i].order = 2 * maxn;
    int valid = 0, removed = 0, p = s1, data;
    while(p != -1) {
        data = abs(node[p].key);
        if(!keyExist[data]) {
            keyExist[data] = true;
            node[p].order = valid++;
        } else {
            node[p].order = maxn + removed++;
        }
        p = node[p].next;
    }
    sort(node, node + maxn, cmp);
    int total = valid + removed;
    for(int i = 0; i < total; i++) {
        if(i != valid - 1 && i != total - 1) 
            printf("%05d %d %05d\n", node[i].addr, node[i].key, node[i + 1].addr);   
        else 
            printf("%05d %d -1\n", node[i].addr, node[i].key);
    }
    return 0;
}

```