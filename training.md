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
这个也是有多种解法，但是还是最容易想到O(n2)的解法。为了训练能有更好的思维方式，所以选择最优雅的解法。那么这里对于两个**链表相交**，可以做假设：
1、若相交:链表A = a + c,链表B = b + c  （c为公共长度）
则 
<div style="text-align: center;">(a+c)+(b+c) = (b+c)+(a+c) &emsp;&emsp;&emsp; //两人走的长度是相同的，所以会在相交点相遇。
</div>
2、若不相交：

则 
<div style="text-align: center;">a+b =b+a   &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; //相当于他们的相交点是null，所以会在null处“相遇”
</div>

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == nullptr || headB == nullptr)
            return nullptr;
        ListNode *pa = headA,*pb = headB;
        while(pa != pb){
            if(pa == nullptr)
                pa = headB;
            else
                pa = pa->next;
            if(pb == nullptr)
                pb = headA;
            else
                pb = pb->next;
            
        }

        return pa;
    }
};
```

#### 3、[反转链表（easy）](https://leetcode.cn/problems/reverse-linked-list/description/)
使用递归，一般都使用这个标准的模板：

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
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        
        ListNode *tmp = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return tmp;
    }
};
```


#### 4、[合并两个有序链表（easy）](https://leetcode.cn/problems/merge-two-sorted-lists/description/)


#### 5、[环形链表（easy）](https://leetcode.cn/problems/linked-list-cycle/description/)
这个还是比较简单能想到的，就是两个跑步的人，跑的快的可以套圈慢的，所以使用快慢指针就可以判断是否会有相交的地方

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr)
            return false;
        
        ListNode *f=head,*s=head;
        while( f != nullptr && f->next != nullptr  ){
            s = s->next;
            f = f->next->next;
            if(f == s)
                return true;
        }

        return false;
    }
};
```

#### 6、[环形链表II（medium）](https://leetcode.cn/problems/linked-list-cycle-ii/description/)
参考前面判断环形链表的模板写出两指针相遇，然后在相遇部分做其他判断：快指针回到表头，慢指针在相遇点出发，后面两者相遇点即为入环节点。
（这里我是没想出来，可以去看题解）

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr)
            return nullptr;
        ListNode *f=head,*s=head;

        while( f != nullptr && f->next != nullptr){
            s = s->next;
            f = f->next->next;
            

            if(f == s){

                f = head;
                while(s != f){
                    s = s->next;
                    f = f->next;
                }
                return s;
            }
        }

        return nullptr;
    }
};
```
