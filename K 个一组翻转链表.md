# K 个一组翻转链表

## 题目：
  给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k的整数倍，那么请将最后剩余的节点保持原有顺序。


思路分析：
        把链表结点按照 k 个一组分组，所以可以使用一个指针 head 依次指向每组的头结点。这个指针每次向前移动 k 步，直至链表结尾。
        对于每个分组，我们先判断它的长度是否大于等于 k。若是，我们就翻转这部分链表，否则不需要翻转。

**实例**

```
给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5
 ```
## 说明
+ 你的算法只能使用常数的额外空间。
+ 你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。




## 算法


    ListNode* reverseKGroup(ListNode* head, int k) {
        if (head == NULL) return NULL;
        ListNode *a = head;
        ListNode *b = head;
        for (int i = 0; i < k; i++) {
            if (b == NULL) return head;
            b = b->next;
        }

        ListNode *newNode = reverseOperator(a,b);
        a->next = reverseKGroup(b,k);
        return newNode;
    }

    ListNode* reverseOperator(ListNode* n,ListNode *b) {
        ListNode *pre, *cur, *nxt;
        pre = NULL; cur = n; nxt = n;
        while (cur != b) {
            nxt = cur->next;
            cur->next = pre;
            pre = cur;
            cur = nxt;
        }
        return pre;
    }
  };

