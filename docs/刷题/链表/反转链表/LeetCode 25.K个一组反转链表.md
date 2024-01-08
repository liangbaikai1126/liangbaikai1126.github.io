**思路**：p0设为上一组反转链表的最后一个结点，设置dummy节点作为哨兵，一步一步局部反转完组内链表后，再完善组之间的反转关系。
```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* cur = head, *pre = NULL, *next, *p0 = dummy;
        int n = 0;
        while(cur) {
            n++;
            cur = cur->next;
        }
        cur = head;
        while(n >= k) {
            n -= k;
            for(int i = 0; i < k; i++) {
                next = cur->next;
                cur->next = pre;
                pre = cur;
                cur = next;
            }
            next = p0->next;
            p0->next->next = cur;
            p0->next = pre;
            p0 = next;
        }
        return dummy->next;
    }
};
```