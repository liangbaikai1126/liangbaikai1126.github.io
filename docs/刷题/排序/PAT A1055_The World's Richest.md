**思路**：本题需要预处理 valid 结构体数组，由于每次查询的人数 M <= 100，让每个年龄的前 100 人进入 valid 数组即可，否则本题在测试点 2 会超时。

```cpp
#include<iostream>
#include<algorithm>
#include<string>
using namespace std;

const int MAXN = 100005;

int N, K;

typedef struct {
    string name;
    int age, worth;
} Info;

Info a[MAXN], valid[MAXN];
int Age[MAXN];

bool cmp(Info i1, Info i2) {
    if(i1.worth != i2.worth) return i1.worth > i2.worth;
    else if(i1.age != i2.age) return i1.age < i2.age;
    return i1.name < i2.name;
}

int main() {
    cin >> N >> K;
    for(int i = 0; i < N; i++) {
        cin >> a[i].name >> a[i].age >> a[i].worth;
    }
    sort(a, a + N, cmp);
    int validNum = 0;
    for(int i = 0; i < N; i++) {
        if(Age[a[i].age] < 100) {
            Age[a[i].age]++;
            valid[validNum++] = a[i];
        }
    }
    int M, AgeL, AgeR;
    for(int q = 1; q <= K; q++) {
        cin >> M >> AgeL >> AgeR;
        cout << "Case #" << q << ":\n";
        int printNum = 0;
        for(int j = 0; j < validNum && printNum < M; j++) {
            if(valid[j].age >= AgeL && valid[j].age <= AgeR) {
                cout << valid[j].name << " " << valid[j].age << " " << valid[j].worth << endl;
                printNum++;
            }
        }
        if(printNum == 0) cout << "None\n";
    }
    return 0;
}
```