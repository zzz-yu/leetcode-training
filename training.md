## 一、链表

#### 1、[回文链表（easy）](https://leetcode.cn/problems/palindrome-linked-list/)
最好想的方法就是用一个相同的数组或者链表复制一份，然后一一对比，但是浪费了空间
所以最好的时间O(n)，空间O(1)的方法是：
**快慢指针+反转链表：**
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;

        //先判断是奇数还是偶数链表
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
        }
        //奇数就是fast->next = nullptr
        //偶数是 fast = nullptr

        //经过上面的寻找，那么slow指针已经到达了链表中间部位（对于奇数链表是len/2，向上取整，偶数是len/2）
        //现在就把一个链表分成了两部分了，一个是前半段，剩下后半段。很容易想到把后半段反转一下就可以和前半段进行对比判断
        ListNode* back = reverse(slow);

        //那么这里开始进行逐步判断
        ListNode* pre = head;
        while(back || back == slow){
            if(pre->val != back->val)
                return false;
            pre = pre->next;
            back = back->next;
        }
        return true;
    }

    ListNode* reverse(ListNode* pre){
        if(!pre || !pre->next)
            return pre;
        ListNode* tmp = reverse(pre->next);
        pre->next->next = pre;
        pre->next = nullptr;
        return tmp;
    }
};
```


#### 2、[相交链表（easy）](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/)
