# Leetcode Linked List

[TOC]

## Split, remove and merge the list

---Set a dummy head, notice the tail 

### Leetcode 21

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode DummyHead = new ListNode(0);
        ListNode curr = DummyHead;
        while (l1 != null && l2 != null){
            if (l1.val<l2.val){
                curr.next = l1;
                l1 = l1.next;
            }
            else{
                curr.next = l2;
                l2 = l2.next;
            }
            curr = curr.next;
        }
        if (l1 == null) {
            curr.next = l2;
        }else {
            curr.next = l1;
        }
        
        return DummyHead.next;
	}
}
```

Attention:

need a dummy head, any var.

curr is the current pointer, draw a list to understand this progress!

### Leetcode 86

Split and join the linked list

 When joining the linked list, pay attention to the **tail** of the list, and remember to **redirect** it to the new node!!!!

```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode dummy1 =  new ListNode(0);
        ListNode dummy2 =  new ListNode(0);
        ListNode curr1 = dummy1; 
        ListNode curr2 = dummy2;
        while (head !=null){
            if (head.val<x){
                curr1.next = head;
                curr1 = curr1.next;
            }
            else{
                curr2.next = head;
                curr2 = curr2.next;
            }
            head = head.next;
        }
        curr2.next = null; //important!!! without this, dummy2 will end with unexpected node! and cause repeation
        curr1.next = dummy2.next;
        return dummy1.next;
    }
}
```

### Leetcode 23

Using minimal heap--priority queue to solve this problem.

initialize the pq: two para, size and comparator

pay attention to the **null condition**!!!

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length ==0) return null;
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, (a, b)->(a.val - b.val));
        for (ListNode head: lists){
            if (head!=null) pq.add(head);
        }
        while(pq.isEmpty()==false){
            ListNode outline = pq.poll();
            if (outline.next !=null) pq.add(outline.next);
            curr.next = outline;
            curr = curr.next;
        }
        return dummy.next;
    }
}
```

### Leetcode 203

set a dummy head for the list

```java
class Solution {
    
    public ListNode removeElements(ListNode head, int val) { 
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode curr = dummy;
        while (curr.next !=null){
            if (curr.next.val == val) curr.next = curr.next.next;
            else curr = curr.next;
        }
        return dummy.next;
    }
}
```



## Traverse the list ---Two pointers (slow and quick)

---cycle or intersect

### Leetcode 19

The core of the solution: two pointers, one goes first, and the other follows

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        ListNode first_cur = head;
        ListNode second_cur = dummy;
        dummy.next = head;
        for (int i=0; i<n; i++){
            first_cur = first_cur.next;
        }
        while (first_cur != null){
            first_cur = first_cur.next;
            second_cur = second_cur.next;
        }
        second_cur.next = second_cur.next.next;
        return dummy.next;      
    }
}
```

### Leetcode 83

just use one pointer:

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode curr=head;
        while(curr!=null &&curr.next!=null)
        {
           if(curr.val==curr.next.val)
           {
               curr.next=curr.next.next;
           }
            else
            {
                curr=curr.next;
            }
        }
        return head;
        
    }
}
```

use two pointers:

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
       if(head == null) return null;
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null){
            if (slow.val !=fast.val){
                slow.next = fast;
                slow = slow.next;
            }
            fast = fast.next;
        }
        slow.next = null;
        return head;
    }
}
```

### Leetcode 876

Also the fast and slow pointer, fast one moves 2, slow one moves 1

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        
        ListNode fast_curr = head;
        ListNode slow_curr = head;
        
        
        while(fast_curr != null && fast_curr.next != null){
            fast_curr = fast_curr.next.next;
            slow_curr = slow_curr.next;
            System.out.println(slow_curr.val);
        }
        return slow_curr;
    }
}
```

### Leetcode 141

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow_curr = head;
        ListNode fast_curr = head;
        while(fast_curr != null && fast_curr.next != null ){
            slow_curr = slow_curr.next;
            fast_curr = fast_curr.next.next;
            if(slow_curr == fast_curr) return true;
        }
        return false;
    }
}
```

### Leetcode 142

the same as the 141, but when two pointers meet, slow point to the head. When the meet again, the position is the cycle

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        boolean flag = false;
        while(fast != null && fast.next!= null){
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast){
                flag =true;
                break;
            }
        }
        if (flag == false) return null;
        else {
            slow = head;
            while(fast != null && slow != fast){
                slow = slow.next;
                fast = fast.next;
            }
            return slow;
                    
            }
        
    }
}
```

### Leetcode 146

traverse A -- B and B -- A, when they meet, is the intersection point

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode currA = headA;
        ListNode currB = headB;
        while (currA != currB){
            if (currA == null) currA = headB;
            else currA = currA.next;
            if (currB == null) currB = headA;
            else currB = currB.next;
            
        }
        return currA;
    }
}
```

## Traverse the list   ---preorder and backorder

### Leetcode 234

To judge if a linked list is palindrome

you can change the list into a array, but this is not the most elegant solution

solve this by recursion, and backorder traverse the list! treat backorder traverse as storing into a stack

Time complexity: O(N), Space complexity: O(N)

```java
class Solution {
    private ListNode left;
    public boolean isPalindrome(ListNode head) {
        left =head;
        return traverse(head);
    }
    public boolean traverse(ListNode right){
        if (right == null ) return true;
        boolean res = traverse(right.next);
        res = res && (right.val == left.val);
        left = left.next;
        return res;
    }
}
```

But we can still solve this by two pointers! and connect with the iteration reverse.

Time complexity: O(N), Space complexity: O(1)

```java
class Solution {
    boolean isPalindrome(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next !=null){
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode prev = null;
        while (slow != null){
            ListNode temp = slow.next;
            slow.next = prev;
            prev = slow;
            slow = temp;
        }
        
        ListNode left = head;
        ListNode right = prev;
        while(right != null){
            if (right.val != left.val) return false; 
            right = right.next;
            left = left.next;
        }
        return true;
    }

}
```

but attention! the **sequence** of the linked list is changed!!!! just reverse it by https://labuladong.github.io/algo/2/19/21/

## Reverse the list

 --- recursion and iteration

### Leetcode 206

reverse the link

amazing **recursion**!!!!! Draw a sketch to get this

When meeting the recursion function, don't focus on what exactly happens in each recursion, just treat it as default, and consider it generally.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null){
            return head;
        }
        ListNode last = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return last;
    }
}
```

And here is the iteration version

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev= null;
        ListNode curr = head;
        while(curr!=null){
            ListNode temp=curr.next;
            curr.next=prev;
            prev=curr;
            curr=temp;
        }
        return prev;
    }
}
```

### Leetcode 92

two steps: reverse the front n list, and all the list 

https://labuladong.github.io/algo/2/19/19/

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if(left == 1){
            return reverse(head,right);
        }
        head.next = reverseBetween(head.next,left-1,right-1);
        
        return head;
    }
    ListNode nextnode = null;
    
  public ListNode reverse(ListNode head, int n){
        if (n == 1){
            nextnode = head.next;
            return head;
        }
        ListNode last = reverse(head.next,n-1);
        head.next.next = head;
        head.next = nextnode;
        return last;
    }
}
```

### Leetcode 25

Here is the solution

[Leetcode 25](./Leetcode 25.md)
