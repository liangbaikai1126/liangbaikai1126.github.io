**思路**：针对每个特定的A，统计其左侧P的数量和右侧T的数量，对答案的贡献为P*T的值。P的数量采用数组，ans在统计T的过程中计算出。

**相似题目**：PAT A1001 Quick Sort
```cpp
#include<cstdio>
#include<cstring>
#define MAXN 100005
#define MOD 1000000007

int leftNum[MAXN];
char str[MAXN];

int main() {
    fgets(str, MAXN, stdin);
    int len = strlen(str);
    if(str[0] == 'P') leftNum[0] = 1;
    for(int i = 1; i < len; i++) {
        leftNum[i] = leftNum[i-1];
        if(str[i] == 'P') leftNum[i]++;
    }
    int rightNum = 0;
    int ans = 0;
    for(int i = len-1; i >= 0; i--) {
        if(str[i] == 'T') {
            rightNum++;
        } else if(str[i] == 'A') {
            ans = (ans + (leftNum[i] * rightNum) % MOD) % MOD;
        }
    }
    printf("%d", ans);
    return 0;
}
```