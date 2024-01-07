**思路**：每轮可分成a组，其中最后一组可能不会人满。将所有人的初始序列输入队列中，每次比赛一共选出a个下一轮选手，
即被淘汰的选手处于a + 1名。下一轮就有a人参赛，根据每组的最大人数可将下一轮的a人继续分组如此循环直到队列中只剩下1人即为排名第一的选手。注意，每个小组的比赛中都提前给参赛选手设下排名为a + 1，不影响后续排名更新。
```cpp
#include<cstdio>
#include<queue>
using namespace std;
const int maxn = 1005;
typedef struct {
    int weight;
    int rank;
} Mouse;
Mouse mouse[maxn];

int main() {
    int N_P, N_G;
    scanf("%d %d", &N_P, &N_G);
    for(int i = 0; i < N_P; i++) {
        scanf("%d", &mouse[i].weight);
    }
    int init_order;
    queue<int> q;
    for(int i = 0; i < N_P; i++) {
        scanf("%d", &init_order);
        q.push(init_order);
    }
    int n = N_P, group;
    while(q.size() != 1) {
        group = n / N_G;
        if(n % N_G) group++;
        for(int i = 0; i < group; i++) {
            int idx = q.front();
            q.pop();
            mouse[idx].rank = group + 1;
            for(int j = 1; j < N_G; j++) {
                if(i * N_G + j >= n) break;
                int front = q.front();
                q.pop();
                mouse[front].rank = group + 1;
                if(mouse[front].weight > mouse[idx].weight) {
                    idx = front;
                }
            }
            q.push(idx);
        }
        n = group;
    }
    mouse[q.front()].rank = 1;
    for(int i = 0; i < N_P; i++) {
        printf("%d", mouse[i].rank);
        if(i < N_P - 1) printf(" ");
    }
    return 0;
}
```