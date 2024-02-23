**思路**：简单的排序题，注意由于分数上限为 100，且分数均不相同，MAXN 取 110。

```cpp
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;

const int MAXN = 110;

int N, l, r;

typedef struct {
    string name, id;
    int grade;
} Student;

Student stu[MAXN];

bool cmp(Student a, Student b) {
    return a.grade > b.grade;
}

int main() {
    cin >> N; 
    for(int i = 0; i < N; i++) {
        cin >> stu[i].name >> stu[i].id >> stu[i].grade;
    }
    cin >> l >> r;
    sort(stu, stu + N, cmp);
    bool isPrint = false;
    for(int i = 0; i < N; i++) {
        if(stu[i].grade >= l && stu[i].grade <= r) {
            cout << stu[i].name << " " << stu[i].id << endl;
            isPrint = true;
        }
    }
    if(!isPrint) cout << "NONE\n";
    return 0;
}
```