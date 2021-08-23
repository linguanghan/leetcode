## 总结（LeetCode）

### （一）链表

#### 1、删除链表中重复的元素

##### [ 1.1、删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

不会把重复的元素删除

递归：

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }

        if(head.val == head.next.val){
            head = deleteDuplicates(head.next);
        }else{
            head.next = deleteDuplicates(head.next);
        }

        return head;
    }
}
```

##### [1.2、删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

会把重复的删除

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }

        if(head.val == head.next.val){
            while(head.next!=null && head.val == head.next.val){
                head = head.next;
            }

            head = deleteDuplicates(head.next);
        }else{
            head.next = deleteDuplicates(head.next);
        }

        return head;
    }

}
```

#### 2、反转链表



### （二）排序算法

#### 1、堆排序

