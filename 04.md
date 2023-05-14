# Day4 LinkedList02

## Related Questions:
### [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)
老师有不同的解法，二刷看看
```
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null){
            return head;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode slow_prev = dummy;
        ListNode slow = head;
        ListNode fast = head.next;

        while(fast != null){
            ListNode fast_next = fast.next;
            slow_prev.next = fast;
            fast.next = slow;
            slow.next = fast_next;

            slow_prev = slow;
            slow = slow_prev.next;
            if (slow != null){
                fast = slow.next;
            }
            else{
                break;
            }
            
        }

        return dummy.next;
    }
}
```

#### [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/):  
```
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null){
            return headA;
        }

        ListNode start = headA;
        while (start.next != null){
            start = start.next;
        }

        start.next = headB;
        ListNode intersection = helper(headA);
        start.next = null;
        return intersection;
    }

    private ListNode helper(ListNode head){
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode slow = dummy.next;
        ListNode fast = dummy.next.next;

        while (slow != fast){
            if (fast == null || fast.next == null){
                return null;
            }
            slow = slow.next;
            fast = fast.next.next;
        }

        slow = dummy;
        while (slow != fast){
            slow = slow.next;
            fast = fast.next;
        }

        return slow;
    }
}
```

#### [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)
```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode fast = head;
        for (int i = 0; i < n; i++){
            if (fast == null){
                return null;
            }
            fast = fast.next;
        }

        ListNode prev = null;
        ListNode slow = head;
        
        if (fast == null){
            return head.next;
        }

        while (fast != null){
            prev = slow;
            slow = slow.next;
            fast = fast.next;
        }

        ListNode node_prev = prev;
        ListNode node_next = slow.next;

        node_prev.next = node_next;
        return head;
    }
}
```

####[142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/)
```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null){
            return null;
        }

        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode fast = dummy.next.next;
        ListNode slow = dummy.next;

        while (fast != slow){
            if (fast == null || fast.next == null){
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
        }

        slow = dummy;

        while (slow != fast){
            slow = slow.next;
            fast = fast.next;
        }

        return slow;
    }
}
```
