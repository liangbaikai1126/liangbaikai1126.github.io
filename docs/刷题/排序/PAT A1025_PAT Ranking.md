**思路**：排序标准为，分数不同时，按分数从大到小进行排序。注意全局排序和局部排序时，分数相同排名也相同，但按照准考证从小到大进行输出。

```cpp
#include<cstring>
#include<cstdio>
#include<algorithm>
using namespace std;

typedef struct Student {
    char id[15];
    int score;
    int location_num;
    int local_rank;
} Student;

Student stu[30010];

bool cmp(Student a, Student b) {
     if(a.score != b.score) {
         return a.score > b.score;
     }
    return strcmp(a.id, b.id) < 0;
}

int main() {
    int n, num = 0;
    scanf("%d", &n);
    int k;
    for(int i = 1; i <= n; i++) {
        scanf("%d", &k);
        for(int j = 0; j < k; j++) {
            scanf("%s %d", stu[num].id, &stu[num].score);
            stu[num].location_num = i;
            num++;
        }
        sort(stu + num - k, stu + num, cmp);
        stu[num-k].local_rank = 1;
        for(int i = 1; i < k; i++) {
            if(stu[num-k+i].score == stu[num-k+i-1].score) {
                stu[num-k+i].local_rank = stu[num-k+i-1].local_rank;
            } else {
                stu[num-k+i].local_rank = i + 1;
            }
        }
    }
    printf("%d\n", num);
    sort(stu, stu + num, cmp);
    int global_rank = 1;
    printf("%s %d %d %d\n", stu[0].id, global_rank, stu[0].location_num, stu[0].local_rank);
    for(int i = 1; i < num; i++) {
        if(stu[i].score != stu[i-1].score) {
            global_rank = i + 1;
        }
        printf("%s %d %d %d\n", stu[i].id, global_rank, stu[i].location_num, stu[i].local_rank);
    }
    return 0;
}
```