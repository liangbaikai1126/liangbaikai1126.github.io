**思路**：根据四种成绩进行四次排序，使用 Rank 数组保存这四种排序中最小的数字作为查询结果。注意该相同分数时排名相同。

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

const int MAXN = 2005;

typedef struct {
    int id;
    int grade[4];
} Student;

Student stu[MAXN];
int Rank[1000000][4], now;
char mp[4] = {'A', 'C', 'M', 'E'};

int N, M;

bool cmp(Student a, Student b) {
    return a.grade[now] > b.grade[now];
}

int main() {
    cin >> N >> M;
    for(int i = 0; i < N; i++) {
        cin >> stu[i].id >> stu[i].grade[1] >> stu[i].grade[2] >> stu[i].grade[3];
        stu[i].grade[0] = stu[i].grade[1] + stu[i].grade[2] +stu[i].grade[3];
    }
    for(now = 0; now < 4; now++) {
        sort(stu, stu + N, cmp);
        Rank[stu[0].id][now] = 1;
        for(int i = 1; i < N; i++) {
            if(stu[i].grade[now] == stu[i - 1].grade[now]) {
                Rank[stu[i].id][now] = Rank[stu[i - 1].id][now];
            } else {
                Rank[stu[i].id][now] = i + 1;
            }
        }
    }
    int q;
    for(int i = 0; i < M; i++) {
        cin >> q;
        if(Rank[q][0] == 0) {
            cout << "N/A\n";
        } else {
            int k = 0;
            for(int j = 0; j < 4; j++) {
                if(Rank[q][j] < Rank[q][k]) {
                    k = j;
                }
            }
            cout << Rank[q][k] << " " << mp[k] << endl;
        }
    }
    return 0;
}
```