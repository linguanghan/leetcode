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

##### [ 2.1、反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

递归

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }

        ListNode last = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return last;
    }
}
```

非递归递归 

`pre null`， `next `先行

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }
        
        ListNode pre = null;
        ListNode cur = head;
        ListNode nxt = head;

        while(cur!=null){
            nxt = cur.next;
            cur.next = pre;
            pre = cur;
            cur = nxt;
        }

        return pre;
    }
}
```

##### [ 2.2、反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if(head == null || head.next == null){
            return head;
        }

        ListNode pre = new ListNode(0);
        pre.next = head;

        //被截断的左节点的前一个节点
        ListNode leftPre = pre;
        //被截断的右节点
        ListNode rightPre = pre;

        for(int i = 0;i < right;i++){
            if(i < left - 1){
                leftPre = leftPre.next;
            }
            rightPre = rightPre.next;
        }

        //记录被截断的右节点的后一个界节点
        ListNode rightAfter = rightPre.next;
        ListNode rightNode = leftPre.next;
        rightPre.next = null;
        //反转链表
        leftPre.next = reverse(leftPre.next);
        rightNode.next = rightAfter;

        return pre.next;

    }

    public ListNode reverse(ListNode head){
        if(head == null || head.next == null){
            return head;
        }

        ListNode last = reverse(head.next);
        head.next.next = head;
        head.next = null;
        return last;
    }
}
```

##### [2.3、K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if(head==null||head.next==null){
            return head;
        }        

        ListNode p = head;
        ListNode q = head;

        for(int i = 0;i < k;i++){
            if(q == null){
                return head;
            }

            q = q.next;
        }
        //创建新节点
        ListNode newHead = reversePQ(p, q);
        //从节点Q开始反转k个
        p.next = reverseKGroup(q, k);
        return newHead;
    }

    public ListNode reversePQ(ListNode p, ListNode q){
        ListNode pre = null;
        ListNode cur = p;
        ListNode nxt = q;

        while(cur != q){
            nxt = cur.next;
            cur.next = pre;
            pre = cur;
            cur = nxt;
        }

        return pre;
    }
}
```

#### 3、环形链表（快慢指针）

##### [3.1、环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null){
            return false;
        }

        ListNode fast = head;
        ListNode slow = head;
	
        while(fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast){
                return true;
            }
        }

        return false;
    
    }
}
```

##### [3.2、环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null || head.next == null){
            return null;
        }

        ListNode fast = head;
        ListNode slow = head;

        int flag = 0;
        while(fast.next != null && fast.next.next != null){
            fast = fast.next.next;
            slow = slow.next;

            if(fast == slow){
                fast = head;
                while(fast != slow){
                    fast = fast.next;
                    slow = slow.next;
                }

                return fast;
            }
        }

        return null;
    }
}
```

#### 4、合并有序链表

##### [4.1、合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null || l2 == null){
            return l1 == null ? l2 : l1;
        }

        if(l1.val <= l2.val){
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }else{
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```

##### [4,2、合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

归并+合并两个有序链表

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists == null || lists.length == 0){
            return null;
        }

        if(lists.length == 1){
            return lists[0];
        }

        if(lists.length == 2){
            return mergeTwo(lists[0], lists[1]);
        }

        int left = 0;
        int right = lists.length - 1;
        int mid = left + (right - left) / 2;
        ListNode[] l1 = new ListNode[mid];
        ListNode[] l2 = new ListNode[right - mid + 1];
        for(int i = 0;i < mid;i++){
            l1[i] = lists[i];
        }

        for(int j = mid;j <= right;j++){
            l2[j - mid] = lists[j];
        }

        return mergeTwo(mergeKLists(l1), mergeKLists(l2));
    }

    public ListNode mergeTwo(ListNode l1, ListNode l2){
        if(l1 == null || l2 == null){
            return l1 == null ? l2 : l1;
        }

        if(l1.val <= l2.val){
            l1.next = mergeTwo(l1.next, l2);
            return l1;
        }else{
            l2.next = mergeTwo(l1, l2.next);
            return l2;
        }
    } 
}
```

#### 5、双指针

##### [5.1 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        if(head == null || head.next == null){
            return head;
        }

        ListNode pre = head;
        ListNode ahead = head;

        for(int i = 0;i < k - 1;i++){
            ahead = ahead.next;
        }

        while(ahead.next != null){
            pre = pre.next;
            ahead = ahead.next;
        }

        return pre;
    }
}
```

##### [5.2、删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode pre = new ListNode(0);
        pre.next = head;

        ListNode p1 = pre;
        ListNode p2 = pre;

        for(int i = 0;i < n;i++){
            p2 = p2.next;
        }

        while(p2.next != null){
            p1 = p1.next;
            p2 = p2.next;
        }

        p1.next = p1.next.next;

        return pre.next;
    }
}
```

##### [ 5.3、相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode l1 = headA;
        ListNode l2 = headB;

        while(l1 != l2){
            l1 = l1 == null ? headB : l1.next;
            l2 = l2 == null ? headA : l2.next;
        }

        return l1;
    }
}
```

### （二）排序算法

#### 1、快排（最优时间复杂度O(log(n))、最差O(n^2)、不稳定）

```java
public void quickSort(int[] nums, int left, int right){
    if(left >= right){
        return;
    }

    int leftIndex = left;
    int rightIndex = right;
    int key = nums[left];

    while(left < right){
        //后面等号容易漏
        while(left < right && nums[right] >= key){
            right--;
        }    
        nums[left] = nums[right];
		//后面等号容易漏
        while(left < right && nums[left] <= key){
            left++;
        }
        nums[right] = nums[left];
    }

    nums[left] = key;

    quickSort(nums, leftIndex, left - 1);
    quickSort(nums, right + 1, rightIndex);
}
```

#### 2、归并排序

```java

```

#### 3、堆排（不稳定）

**建堆时间复杂度（O(N)）**

n个节点的堆，树高度是h=floor(log n)。

对深度为于h-1层的节点，比较2次，交换1次，这一层最多有2^(h-1)个节点，总共操作次数最多为3(12^(h-1))；对深度为h-2层的节点，总共有2^(h-2)个，每个节点最多比较4次，交换2次，所以操作次数最多为3(22^(h-2))……
以此类推，从最后一个父节点到根结点进行堆调整的总共操作次数为：

```clike
s=3*[2^(h-1) + 2*2^(h-2) + 3*2^(h-3) + … + h*2^0]       a
2s=3*[2^h + 2*2^(h-1) + 3*2(h-2) + … + h*2^1]           b
b-a，得到一个等比数列，根据等比数列求和公式
s = 2s - s = 3*[2^h + 2^(h-1) + 2^(h-2) + … + 2 - h]=3*[2^(h+1)-  2 - h]≈3*n
```

所以建堆的时间复杂度是O(n)。

**调整堆的时间复杂度（O(log(N))）**

从堆调整的代码可以看到是当前节点与其子节点比较两次，交换一次。父节点与哪一个子节点进行交换，就对该子节点递归进行此操作，设对调整的时间复杂度为T(k)（k为该层节点到叶节点的距离），那么有：

- T(k)=T(k-1)+3, k∈[2,h]
- T(1)=3

迭代法计算结果为：

- `T(h)=3h=3floor(log n)`

所以堆调整的时间复杂度是`O(log n) `。

```java
class Solution {
    public int[] sortArray(int[] nums) {
        if(nums == null || nums.length == 0){
            return new int[0];
        }
        
        int heapSize = nums.length;
        bulidHeap(nums, heapSize);

        for(int i = nums.length - 1;i >= 1;i--){
            swap(nums, 0, i);
            heapSize--;
            maxHeapfy(nums, 0, heapSize);
        }
        return nums;
    }

    public void bulidHeap(int nums[], int heapSize){
        for(int i = nums.length/2; i >= 0;i--){
            maxHeapfy(nums, i, heapSize);
        }
    }

    public void maxHeapfy(int[] nums, int i, int heapSize){
        int l = 2 * i + 1;
        int r = 2 * i + 2;
        int largest = i;

        if(l < heapSize && nums[l] > nums[largest]){
            largest = l;
        }

        if(r < heapSize && nums[r] > nums[largest]){
            largest = r;
        }

        if(i != largest){
            swap(nums, i, largest);
            maxHeapfy(nums, largest, heapSize);
        }
    }

    public void swap(int[] nums, int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

