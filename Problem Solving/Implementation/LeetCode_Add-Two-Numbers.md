### 문제 링크
---
https://leetcode.com/problems/add-two-numbers/

### 풀이
---
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

        ListNode output = new ListNode(0);
        ListNode outputHead = output;
        int sum = 0;
        int carry = 0;
        int x, y;
        while(l1 != null || l2 != null){

            x = (l1 != null) ? l1.val : 0;
            y = (l2 != null) ? l2.val : 0;

            sum = carry + x + y;
            output.next = new ListNode(sum % 10);
            carry = sum / 10;
            output = output.next;

            // next
            if(l1 != null) l1 = l1.next;
            if(l2 != null) l2 = l2.next;
        }

        if(0 < carry){
            output.next = new ListNode(carry);
        }

        return outputHead.next;
    }
}
```
