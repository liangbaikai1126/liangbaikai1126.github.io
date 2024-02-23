**思路**：本题需要将学生和学校信息封装成结构，对成绩排序成排名后遍历每个学生进行遍历，检查其志愿学校是否还有空余名额或者最后一名录取排名与当前学生相同，则录取，否则落榜。

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

const int MAXN = 40005;
const int MAXM = 105;
const int MAXK = 10;

int N, M, K;

typedef struct {
    int ge, gi, sum, rank, sid;
    int offer[MAXK];
} Student;

typedef struct {
    int quota, stuNum, lastAdmit;
    int aid[MAXN];
} School;

Student stu[MAXN];
School sch[MAXM];

bool cmpStu(Student a, Student b) {
    if(a.sum != b.sum) return a.sum > b.sum;
    else return a.ge > b.ge;
}

bool cmpID(int a, int b) {
    return stu[a].sid < stu[b].sid;
}

int main() {
    cin >> N >> M >> K;
    for(int i = 0; i < M; i++) {
        cin >> sch[i].quota;
        sch[i].stuNum = 0;
        sch[i].lastAdmit = -1;
    }
    for(int i = 0; i < N; i++) {
        cin >> stu[i].ge >> stu[i].gi;
        stu[i].sum = stu[i].ge + stu[i].gi;
        for(int j = 0; j < K; j++) {
            cin >> stu[i].offer[j];
        }
        stu[i].sid = i;
    }
    sort(stu, stu + N, cmpStu);
    for(int i = 0; i < N; i++) {
        if(i > 0 && stu[i].sum == stu[i - 1].sum && stu[i].ge == stu[i - 1].ge) {
            stu[i].rank = stu[i - 1].rank;
        } else {
            stu[i].rank = i;
        }
    }
    for(int i = 0; i < N; i++) {
        for(int j = 0; j < K; j++) {
            int offer = stu[i].offer[j];
            int nowNum = sch[offer].stuNum;
            int last = sch[offer].lastAdmit;
            if(nowNum < sch[offer].quota || (last != -1 && stu[i].rank == stu[last].rank)) {
                sch[offer].aid[nowNum] = i;
                sch[offer].lastAdmit = i;
                sch[offer].stuNum++;
                break;
            }
        }
    }
    for(int i = 0; i < M; i++) {
        if(sch[i].stuNum > 0) {
            sort(sch[i].aid, sch[i].aid + sch[i].stuNum, cmpID);
            for(int j = 0; j < sch[i].stuNum; j++) {
                cout << stu[sch[i].aid[j]].sid;
                if(j < sch[i].stuNum - 1) cout << " ";
            }
        }
        cout << endl;
    }
    return 0;
}
```