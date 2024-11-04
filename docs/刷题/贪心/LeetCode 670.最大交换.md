只交换一次让所得数字最大，即将靠左的小数字与靠右的大数字交换

1. 从右往左寻找第一个最大的数字，记录下标 maxIdx，即大数字优先靠右
2. 遍历的同时查看 num[i] 是否小于 num[maxIdx]，即小数字优先靠左
3. 若 num[i] < num[maxIdx]，记录这两个下标保存在 p，q 中
4. i 需要越靠左越好，maxIdx 需要越靠右越好
5. 若数字为降序排序，则未发生交换，可将 p，q 初始化为 n - 1 来免去特殊判断

```python
class Solution:
    def maximumSwap(self, num: int) -> int:
        num_str = str(num)
        p = q = max_idx = len(num_str) - 1
        for i in range(len(num_str) - 2, -1, -1):
            if num_str[i] > num_str[max_idx]:
                max_idx = i
            elif num_str[i] < num_str[max_idx]:
                p, q = i, max_idx
        num_list = list(num_str)
        num_list[p], num_list[q] = num_list[q], num_list[p]
        return int(''.join(num_list))
```