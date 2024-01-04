**思路**：由于题目所给结点中键值唯一，可直接根据键值排序，注意输入中的结点可能并不属于链表，需要在Node中添加flag标志查看是否处于题目的链表中，并根据此编写cmp函数将其排序到末尾。

```cpp
#include<cstdio>
#include<algorithm>
using namespace std;

const int maxn = 100005;

typedef struct {
    int addr;
    int data;
    int next;
    bool flag;
} Node;

Node node[maxn];

bool cmp(Node a, Node b) {
    if(a.flag == false || b.flag == false) {
        return a.flag > b.flag;
    }
    return a.data < b.data;
}

int main() {
    int n, s1;
    scanf("%d %d", &n, &s1);
    int addr, data, next;
    for(int i = 0; i < n; i++) {
        scanf("%d %d %d", &addr, &data, &next);
        node[addr].addr = addr;
        node[addr].data = data;
        node[addr].next = next;
        node[addr].flag = false;
    }
    int p = s1, cnt = 0;
    while(p != -1) {
        node[p].flag = true;
        p = node[p].next;
        cnt++;
    }
    if(cnt == 0) printf("0 -1\n");
    else {
        sort(node, node + maxn, cmp);
        printf("%d %05d\n", cnt, node[0].addr);
        for(int i = 0; i < cnt; i++) {
            if(i < cnt-1) {
                printf("%05d %d %05d\n", node[i].addr, node[i].data, node[i+1].addr);
            } else {
                printf("%05d %d -1\n", node[i].addr, node[i].data);
            }
        }
    }
    return 0;
}
```