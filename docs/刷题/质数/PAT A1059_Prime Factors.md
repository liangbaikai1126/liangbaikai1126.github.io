**思路**：对int范围内的整数进行质因数分解，采用打质数表法。
注意由于前10个质数乘积已经超过int最大值范围，只需要开辟Factors大小为10来记录每个质数及其重复次数。
而由于质因子关于sqrt(n)的对称性质，若当前质因子>sqrt(n)时，可判断n本身即为质数。

```cpp
#include<cstdio>

const int maxn = 100010;

int prime[maxn];
int pnum = 0;

bool is_prime(int x) {
    if(x <= 1) return false;
    for(int i = 2; i*i <= x; i++) {
        if(x % i == 0) return false;
    }
    return true;
}

void FindPrime() {
    for(int i = 2; i < maxn; i++) {
        if(is_prime(i) == true) {
            prime[pnum++] = i;
        }
    }
}

typedef struct {
    int x;
    int cnt;
} Factors;

Factors fac[10];

int main() {
    FindPrime();
    int n, num = 0;
    scanf("%d", &n);
    if(n == 1) {
        printf("1=1");
        return 0;
    }
    printf("%d=", n);
    for(int i = 0; i < pnum && prime[i] * prime[i] <= n; i++) {
        if(n % prime[i] == 0) {
            fac[num].x = prime[i];
            fac[num].cnt = 0;
            while(n % prime[i] == 0) {
                fac[num].cnt++;
                n /= prime[i];
            }
            num++;
        }
    }
    if(n > 1) {
        fac[num].x = n;
        fac[num++].cnt = 1;
    }
    for(int i = 0; i < num; i++) {
        printf("%d", fac[i].x);
        if(fac[i].cnt > 1)
            printf("^%d", fac[i].cnt);
        if(i < num - 1) printf("*");
    }
    return 0;
}
```