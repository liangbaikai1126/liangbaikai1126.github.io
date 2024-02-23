**思路**：本题进行排序时，注意一条提交记录中，if 和 else if 的判断容易混淆，使用 if 更方便进行各种条件的判断。

此外，使用 flag 可以标志是否需要输出，而非通过单独判断 score 是否全为 0 或 -1。因为本题中第一次提交如果编译未通过，需要把分数赋值为 0。故只有当提交记录中的得分 >= 0 时，才可以设置 flag == true。

由于提交满分后仍会有低分提交出现，在处理输入时，需要设置最高得分为本题最终得分，最后单独统计总分。

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
using namespace std;

const int MAXN = 10005;

typedef struct {
    int id, totalScore, solve;
    int score[6];
    bool flag;
} Student;

Student stu[MAXN];

int N, K, M, val[6];

void init() {
    for(int i = 1; i <= N; i++) {
        stu[i].id = i;
        stu[i].totalScore = 0;
        stu[i].solve = 0;
        stu[i].flag = false;
        fill(stu[i].score, stu[i].score + 6, -1);
    }
}

bool cmp(Student a, Student b) {
    if(a.totalScore != b.totalScore) return a.totalScore > b.totalScore;
    else if(a.solve != b.solve) return a.solve > b.solve;
    return a.id < b.id;
}

int main() {
    cin >> N >> K >> M;
    init();
    for(int i = 1; i <= K; i++)
        cin >> val[i];
    int id, index, score;
    for(int i = 0; i < M; i++) {
        cin >> id >> index >> score;
        if(score != -1) {
            if(score == val[index] && stu[id].score[index] < val[index]) {
                stu[id].solve++;
            }
            if(score > stu[id].score[index]) {
                stu[id].score[index] = score;
            }
            stu[id].flag = true;
        } else if(score == -1 && stu[id].score[index] == -1) {
            stu[id].score[index] = 0;
        }
    }
    for(int i = 1; i <= N; i++) {
        for(int j = 1; j <= K; j++) {
            if(stu[i].score[j] != -1) {
                stu[i].totalScore += stu[i].score[j];
            }
        }
    }
    sort(stu + 1, stu + 1 + N, cmp);
    int rank = 1;
    for(int i = 1; i <= N && stu[i].flag; i++) {
        if(i > 1 && stu[i].totalScore != stu[i - 1].totalScore) {
            rank = i;
        } 
        printf("%d %05d %d", rank, stu[i].id, stu[i].totalScore);
        for(int j = 1; j <= K; j++) {
            if(stu[i].score[j] != -1) {
                cout << " " << stu[i].score[j];
            } else {
                cout << " -";
            }
        }
        cout << endl;
    }
    return 0;
}
```