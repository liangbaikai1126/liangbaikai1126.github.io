只交换一次让字典序最小，需要满足下列条件

1. 奇偶性相同，即绝对值之差等于 2
2. 交换前的数字必须左边大于右边，交换后才能字典序变小
3. 交换的位置越靠近高位越好，即越靠近左边越好

```python
class Solution:
    def getSmallestString(self, s: str) -> str:
        t = list(s)
        for i in range(0, len(t) - 1):
            x, y = t[i], t[i + 1]
            if x > y and (ord(x) - ord(y)) % 2 == 0:
                t[i], t[i + 1] = y, x
                break
        return ''.join(t)
```