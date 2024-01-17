**思路**：本题考察堆排序的具体过程。注意题目给出初始序列不包含在中间序列里。堆排序时，若要进行递增排序，首先应该从最后一个非叶结点开始，将其与键值更大的子节点进行互换调整，类似于建立大根堆。建堆之后，每次将堆顶元素放到堆的末尾，将堆大小减一，重复这个过程直到堆的大小为1，排序完成。
```cpp
#include<cstdio>
#include<algorithm>

using namespace std;

const int maxn = 105;

int origin[maxn];
int temp[maxn];
int intermediate[maxn];
int n;

void printArray(int a[], int size) {
    for(int i = 1; i <= size; i++) {
        printf("%d", a[i]);
        if(i < size) printf(" ");
    }
}

bool isArrEqual(int a[], int b[], int size) {
    for(int i = 1; i <= size; i++)
        if(a[i] != b[i]) return false;
    return true;
}

bool isInsert() {
    bool flag = false;
    for(int i = 2; i <= n; i++) {
        if(i != 2 && isArrEqual(temp, intermediate, n)) 
            flag = true;
        sort(temp + 1, temp + i + 1);
        if(flag) return true;
    }
    return false;
}

void downAdjust(int low, int high) {
    int i = low, j = 2 * i;
    while(j <= high) {
        if(j + 1 <= high && temp[j + 1] > temp[j])
            j = j + 1;
        if(temp[j] > temp[i]) {
            swap(temp[i], temp[j]);
            i = j;
            j = 2 * i;
        } else  {
            break;
        }
    }
}

void HeapSort() {
    for(int i = 1; i <= n; i++)
        temp[i] = origin[i];
    for(int i = n / 2; i >= 1; i--)
        downAdjust(i, n);
    bool flag = false;
    for(int i = n; i > 1; i--) {
        if(i != n && isArrEqual(temp, intermediate, n))
            flag = true;
        swap(temp[i], temp[1]);
        downAdjust(1, i - 1);
        if(flag) return;
    }
}

int main() {
    scanf("%d", &n);
    for(int i = 1; i <= n; i++) {
        scanf("%d", &origin[i]);
        temp[i] = origin[i];
    }
    for(int i = 1; i <= n; i++)
        scanf("%d", &intermediate[i]);
    if(isInsert()) {
        printf("Insertion Sort\n");
        printArray(temp, n);
    } else {
        printf("Heap Sort\n");
        HeapSort();
        printArray(temp, n);
    }
    return 0;
}
```