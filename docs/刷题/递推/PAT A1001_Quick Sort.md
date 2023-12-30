**思路**：与A1093相似，针对每个数统计其左边的最大值以及右边的最小值。
如果满足当前数大于左边最大值且小于右边最小值，则满足条件。
最大值用数组统计，最小值只设变量rightMin，在统计rightMin的循环中统计满足要求的下标进而得出答案。
```cpp
#include<cstdio>
#define MAXN 100005

int nums[MAXN];
int leftMAX[MAXN];
int ans[MAXN];

int main() {
    int n;
    scanf("%d", &n);
    for(int i = 0; i < n; i++) {
        scanf("%d", &nums[i]);
    }
    leftMAX[0] = nums[0];
    for(int i = 1; i < n; i++) {
        leftMAX[i] = leftMAX[i-1];
        if(nums[i] > leftMAX[i]) {
            leftMAX[i] = nums[i];
        }
    }
    int count = 0;
    int rightMin = nums[n-1];
    for(int i = n-1; i >= 0; i--) {
        if(nums[i] <= rightMin) {
            rightMin = nums[i];
            if(nums[i] >= leftMAX[i]) {
                ans[count++] = i;
            }
        }
    }
    printf("%d\n", count);
    for(int i = count-1; i >= 0; i--) {
        printf("%d", nums[ans[i]]);
        if(i > 0) printf(" ");
    }
    printf("\n");
    return 0;
}
```