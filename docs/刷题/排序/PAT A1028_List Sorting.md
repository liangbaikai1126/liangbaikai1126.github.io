**思路**：根据输出的三种 C 的取值编写三个排序 bool 函数即可。

```cpp
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;

const int MAXN = 100005;

typedef struct {
    int id, score;
    string name;
} Student;

Student stu[MAXN];

int N, C;

bool cmp1(Student a, Student b) {
    return a.id < b.id;
}

bool cmp2(Student a, Student b) {
    if(a.name != b.name) return a.name < b.name;
    return a.id < b.id;
}

bool cmp3(Student a, Student b) {
    if(a.score != b.score) return a.score < b.score;
    return a.id < b.id;
}

int main() {
    cin >> N >> C;
    for(int i = 0; i < N; i++) 
        cin >> stu[i].id >> stu[i].name >> stu[i].score;
    if(C == 1) sort(stu, stu + N, cmp1);
    else if(C == 2) sort(stu, stu + N, cmp2);
    else if(C == 3) sort(stu, stu + N, cmp3);
    for(int i = 0; i < N; i++) {
        printf("%06d ", stu[i].id);
        cout << stu[i].name << " " << stu[i].score << endl;
    }
    return 0;
}
```