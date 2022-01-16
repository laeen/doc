# list专题

## [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

https://leetcode.com/problems/merge-two-sorted-lists/

本题目的是初步学习一下二级指针，二级指针在进行链表的替换，删除操作的时候，非常好用。

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* ret = NULL;
        ListNode** cur = &ret;
        
        while(list1 && list2){
       
            if(list1->val <= list2->val){
                *cur = list1;
                 cur = &((*cur)->next);
                list1 = list1->next;
            }else{
                *cur = list2;
                 cur = &((*cur)->next);
                list2 = list2->next;
            }

        }
        *cur = list1 ? list1 : list2;
        return ret;
    }
};
```


## [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)


https://leetcode.com/problems/remove-nth-node-from-end-of-list/

这一题在删除节点的基础上，增加了双指针的应用


```c++
class Solution {

public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        
        ListNode *p = head;
        ListNode **cur = &head;
        
        for(int i = 0 ; i < n && p ; ++i) p = p->next;
        
        while(p){
            cur = &((*cur)->next);
            p = p->next;
        }
            
        (*cur) = (*cur)->next;
        
        return head;
    }
    
};
```

## [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/

这次有点难度，但是如果能熟练使用二级指针，就很容可以删除。

要判断一个节点是否需要删除，只需要判断是否有重复即可。

```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode *ret = head;
        ListNode **cur = &ret;
        ListNode *p = NULL;
        while((*cur) && (*cur)->next){
            //当存在相同的指针
            if((*cur)->val == (*cur)->next->val){
                int key = (*cur)->val;
                p = (*cur)->next;
                // skip 相同的数字
                while(p && p->val == key){
                    p = p->next;
                }
                *cur = p;
            }else{
                cur = &((*cur)->next);
            }
        }
        
        return ret;
    }
};

```