**思路**：将输入的链表用静态链表存储，对第一条链表的所有结点采用flag标记，在遍历第二条链表时检查该标记，出现标记即为第一个重合的结点，否则两个链表不存在重合的结点。
```cpp
#include<cstdio>
#include<cstring>

const int maxn = 100005;

typedef struct {
    bool flag;
    char data;
    int next;
} Node;

Node node [maxn];

int main() {
    int s1, s2, n;
    scanf("%d %d %d", &s1, &s2, &n);
    int addr, next;
    char data;
    for(int i = 0; i < n; i++) {
        scanf("%d %c %d", &addr, &data, &next);
        node[addr].data = data;
        node[addr].next = next;
    }
    while(s1 != -1) {
        node[s1].flag = true;
        s1 = node[s1].next;
    }
    while(s2 != -1) {
        if(node[s2].flag == true) {
            break;
        }
        s2 = node[s2].next;
    }
    if(s2 != -1) {
        printf("%05d", s2);
    } else {
        printf("-1");
    }
    return 0;
}
```