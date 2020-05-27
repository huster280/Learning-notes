

# leetcode刷题笔记之链表

#### 19. 删除链表的倒数第N个节点

难度：[中等](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/ )

**描述**

>  给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。 
>
> 示例：
>
> >   给定一个链表: 1->2->3->4->5 和 n = 2.
> >   当删除了倒数第二个节点后，链表变为 1->2->3->5.

**思路**

> 快慢指针：快指针先走 n 步，然后快慢指针一起走，直到快指针达到链表结尾。

```c++
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode* fast = head;
    ListNode* slow = head;
    if (head == NULL || head->next == NULL)
        return NULL;
    while (n--) 
    {
        fast = fast->next;
    }

    if (fast == NULL)
        return head->next;      //若快指针已经走到最后了，则删除第一个节点

    while (fast->next != NULL)
    {
        fast = fast->next;
        slow = slow->next;
    }
    slow->next = slow->next->next;		//删除节点
    return head;
}
```



#### 21. 合并两个有序链表

难度：[简单](https://leetcode-cn.com/problems/merge-two-sorted-lists/ )

**描述**

>  将两个升序链表合并为一个新的升序链表并返回。 
>
> 示例：
>
> >   输入：1->2->4, 1->3->4
> >   输出：1->1->2->3->4->4

（1）递归版本，时间复杂度：*O(n + m)*，空间复杂度*O(n + m)*（递归调用的栈空间）

```c++
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    if (l1 == NULL)
        return l2;
    if (l2 == NULL)
        return l1;
    if (l1->val <= l2->val) {
        l1->next = mergeTwoLists(l1->next, l2);
        return l1;
    } else {
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
}
```

（2）迭代版本，时间复杂度：*O(n + m)*，空间复杂度*O(1)*

```c++
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    // 新建头节点的前一节点，这样不用判断l1、l2哪个是头节点
    ListNode* newHead = new ListNode(-1);
    ListNode* cur =  newHead;

    while (l1 != NULL && l2 != NULL) {
        if (l1->val <= l2->val) {
            cur->next = l1;
            l1 = l1->next;
        } else {
            cur->next = l2;
            l2 = l2->next;
        }
        cur = cur->next;
    }
    // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
    cur->next = (l1 == NULL) ? l2 : l1;
    return newHead->next;
}
```



#### 206. 反转链表

难度：[简单](https://leetcode-cn.com/problems/reverse-linked-list/ )

**描述**

>  反转一个单链表。 
>
> 示例：
>
> >   输入: 1->2->3->4->5->NULL
> >   输出: 5->4->3->2->1->NULL

（1）递归版本，时间复杂度：*O(n)*，空间复杂度*O(n)*（递归调用的栈空间）

```c++
ListNode* reverseList(ListNode* head) {
    if (head == NULL || head->next == NULL) 
        return head;
    ListNode* p = reverseList(head->next);  // 最深层p首先为5，head为4
    head->next->next = head;    // 4的next是5，再把5指向4，实现反转
    head->next = NULL;
    return p;   // p始终指向尾节点，即新链表的头节点
}
```

（2）迭代版本，时间复杂度：*O(n)*，空间复杂度*O(1)*

```c++
ListNode* reverseList(ListNode* head) {
    // 设置 pre 和 cur 两个指针
    ListNode* pre = NULL;
    ListNode* cur = head;
    // 每次循环，都将当前节点指向它前面的节点，然后当前节点和前节点后移
    while (cur != NULL) {
        ListNode* tmp = cur->next;
        cur->next = pre;
        pre = cur;	// 前指针后移
        cur = tmp;	// 当前指针后移
    }
    return pre;
}
```












