**思路**：由于本题中 M <= 1000，因此 哈希表的大小只需要大于 1000，而非题中给出的 N 的最大值 100000，之后便为经典题两数之和的散列解法。注意到若存在多对答案应该按字典序的优先级进行输出，因此选择从 0 开始遍历而非输入的第一个数字开始进行遍历。

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

const int MAXN = 1005;

int n, m, hashTable[MAXN];

int main() {
    cin >> n >> m;
    int num;
    for(int i = 0; i < n; i++) {
        cin >> num;
        hashTable[num]++;
    }
    for(int i = 0; i < MAXN; i++) {
        if(hashTable[i] && hashTable[m - i]) {
            if(i == m - i && hashTable[i] <= 1)
                continue;
            cout << i << " " << m - i;
            return 0;
        }
    }
    cout << "No Solution";
    return 0;
}
```