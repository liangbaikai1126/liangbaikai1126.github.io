**思路**：先对存储因子的fac数组进行预处理，比如假设 p = 2，可得 fac[0] = 0, fac[1] = 1, fac[2] = 4 等。直接预处理至fac[i] >= n，再通过DFS进行搜索求出最优解。 
```cpp
#include<cstdio>
#include<vector>
using namespace std;

vector<int> fac, ans, res;

int n, k, p, maxFacSum = 0;

int power(int x) {
    int num = 1;
    for(int i = 0; i < p; i++)
        num *= x;
    return num;
}

void DFS(int idx, int numK, int sum, int facSum) {
    if(sum == n && numK == k) {
        if(facSum > maxFacSum) {
            ans = res;
            maxFacSum = facSum;
        }
        return;
    }
    if(sum > n || numK > k) return;
    if(idx >= 1) {
        res.push_back(idx);
        DFS(idx, numK + 1, sum + fac[idx], facSum + idx);
        res.pop_back();
        DFS(idx - 1, numK, sum, facSum);
    }
}

int main() {
    scanf("%d %d %d", &n, &k, &p);
    int facP = 0, cnt = 0;
    while(facP <= n) {
        fac.push_back(facP);
        facP = power(++cnt);
    }
    DFS(fac.size() - 1, 0, 0, 0);
    if(maxFacSum == 0)
        printf("Impossible");
    else {
        printf("%d = %d^%d", n, ans[0], p);
        for(int i = 1; i < ans.size(); i++)
            printf(" + %d^%d", ans[i], p);
    }
    return 0;
}
```