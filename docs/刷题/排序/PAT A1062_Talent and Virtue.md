**思路**：输入可分为 1. 德才 >= H  2. 德 >= H，才 < H  3. 德 >= 才  4. 德才 >= L  5. 德或才 < L 共 5 档

此外，先按档次，其次总分，再按德分，最后按 id，以此为依据进行排序。

```cpp
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;

const int MAXN = 100005;

typedef struct {
    string id;
    int vg, tg, sum, flag;
} Student;

Student stu[MAXN];

int N, L, H;

bool cmp(Student a, Student b) {
    if(a.flag != b.flag) return a.flag < b.flag;
    else if(a.sum != b.sum) return a.sum > b.sum;
    else if(a.vg != b.vg) return a.vg > b.vg;
    return a.id < b.id;
}

int main() {
    cin >> N >> L >> H;
    int M = N;
    for(int i = 0; i < N; i++) {
        cin >> stu[i].id >> stu[i].vg >> stu[i].tg;
        stu[i].sum = stu[i].vg + stu[i].tg;
        if(stu[i].vg < L || stu[i].tg < L) {
            stu[i].flag = 5;
            M--;
        } else if(stu[i].vg >= H && stu[i].tg >= H) {
            stu[i].flag = 1;
        } else if(stu[i].vg >= H && stu[i].tg < H) {
            stu[i].flag = 2;
        } else if(stu[i].vg >= stu[i].tg) {
            stu[i].flag = 3;
        } else {
            stu[i].flag = 4;
        }
    }
    sort(stu, stu + N, cmp);
    cout << M << endl;
    for(int i = 0; i < M; i++) {
        cout << stu[i].id << " " << stu[i].vg << " " << stu[i].tg << endl;
    }
    return 0;
}
```