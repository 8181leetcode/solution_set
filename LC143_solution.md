# #LC143 重排链表

## 题目描述：
给定一个单链表 L 的头节点 head ，单链表 L 表示为：

L0 → L1 → … → Ln-1 → Ln 

请将其重新排列后变为：

L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …

不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

## 思路一：使用线性表

由于链表无法随机访问，因此我们考虑先使用线性表记录链表，然后再根据题目要求由线性表重建该链表。使用这种方法的话时间复杂度和空间复杂度都将达到O(n)。

**代码**
```cpp
class Solution {
public:
    void reorderList(ListNode *head) {
        if (head == nullptr) {
            return;
        }
        vector<ListNode *> vec;
        ListNode *node = head;
        while (node != nullptr) {
            vec.emplace_back(node);
            node = node->next;
        }
        int i = 0, j = vec.size() - 1;
        while (i < j) {
            vec[i]->next = vec[j];
            i++;
            if (i == j) {
                break;
            }
            vec[j]->next = vec[i];
            j--;
        }
        vec[i]->next = nullptr;
    }
};
```

## 思路二：快慢指针找中点 + 链表翻转

此题的核心问题在于链表的结构不支持**随机访问**，但从题目要求出发，最终链表的构建也并非没有规律：我们可以很容易地想到将链表拆成前半部分和后半部分，然后再轮流地向答案链表中插入元素。只不过需要注意的是后半部分的链表需要进行翻转。这样一来我们就不需要使用额外的空间，只需要O(1)的空间就能完成该题。

在链表中寻找中点的最好方法是使用**快慢指针**：遍历时令快指针走两步，慢指针走一步，则快指针到达链表尾部时慢指针刚好到达链表中点。

**翻转链表**是本题的另一个考点，通过正向迭代和反向插入即可实现链表的翻转，要注意翻转链表时需要用到的**三个指针**的关系。

**代码**
```cpp
public:
    void reorderList(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;
        ListNode* reversed_tail_part;
        ListNode* iter;
        ListNode* iter_for_tail;

        while (true) {
            if (fast == nullptr || fast->next == nullptr) {
                break;
            }
            fast = fast->next->next;
            slow = slow->next;
        }

        reversed_tail_part = nullptr;
        iter = slow->next;

        while(iter != nullptr) {
            ListNode* next_tmp = iter->next;
            iter->next = reversed_tail_part;
            reversed_tail_part = iter;
            iter = next_tmp;
        }

        slow->next = nullptr;
        iter = head;
        iter_for_tail = reversed_tail_part;
        while(iter != nullptr && iter_for_tail != nullptr) {
            ListNode* tmp = iter->next;
            iter->next = iter_for_tail;
            iter_for_tail = iter_for_tail->next;
            iter->next->next = tmp;
            iter = iter->next->next;
        }
    }
};
```

## 总结
快慢指针和链表翻转是链表题里常出现的两个考点，在链表题中最重要的是需要明确“指针的值是某块内存的地址”的想法，在进行链表的操作时才不会导致关系混乱。
