**思路**：简单的输入封装结构模拟。

```cpp
#include<iostream>
using namespace std;

typedef struct {
    string name, id;
    int score;
} Student;

Student m, f, temp;
int n;

void init() {
    m.score = 101;
    f.score = -1;
}

int main() {
    init();
    cin >> n;
    char gender;
    for(int i = 0; i < n; i++) {
        cin >> temp.name >> gender >> temp.id >> temp.score;
        if(gender == 'M' && temp.score < m.score) 
            m = temp;
        else if(gender == 'F' && temp.score > f.score)
            f = temp;
    }
    if(f.score == -1) cout << "Absent" << endl;
    else cout << f.name << " " << f.id << endl;
    if(m.score == 101) cout << "Absent" << endl;
    else cout << m.name << " " << m.id << endl;
    if(f.score == -1 || m.score == 101) cout << "NA";
    else cout << f.score - m.score;
    return 0;
}
```