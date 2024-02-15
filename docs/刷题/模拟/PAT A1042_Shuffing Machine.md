**思路**：按照题目意思进行模拟即可，注意 iostream 头文件中已经有 next 的命名，可能出现报错。

```cpp
#include<cstdio>
using namespace std;

const int N = 54;

char numToLet[5] = {'S', 'H', 'C', 'D', 'J'};
int start[60], end[60], next[60];
int k;

int main() {
    scanf("%d", &k);
    for(int i = 1; i <= N; i++) {
        start[i] = i;
        scanf("%d", &next[i]);
    }
    for(int i = 0; i < k; i++) {
        for(int j = 1; j <= N; j++) {
            end[next[j]] = start[j];
        }
        for(int j = 1; j <= N; j++) {
            start[j] = end[j];
        }
    }
    for(int i = 1; i <= N; i++) {
        printf("%c%d", numToLet[(start[i] - 1) / 13], (start[i] - 1) % 13 + 1);
        if(i != N) printf(" ");
    }
    return 0;
}
```