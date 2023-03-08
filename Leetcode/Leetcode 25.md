# Leetcode 25

Something about the linked list reverse and recursion

reverse the linked list by pair

First think about the whole linked list reverse ---leetcode 206

wrong version for example

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode curr = head;
        ListNode prev = null;
       while(curr != null && curr.next!=null){
           ListNode temp = curr.next;
           System.out.println(curr.next.next.val);
         //here! change the original linked list! temp is a pointer, not a new list!  
         	 temp.next = prev;
           prev = temp;
           curr = curr.next;
           
       }
        return prev;
    }
}
```

So if use iteration, this is a good one, which means break the list in the middle

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode curr = head;
        ListNode prev = null;
       while(curr != null ){
           ListNode temp = curr.next;
           curr.next = prev;
           prev = curr;
           curr = temp;
       }
        return prev;
    }
}
```

and for q25, it can be divided into 2 part: find the base case and reverse the list. 

For the base case, when the rest of list less than k, return the head

```java
class Solution { 
    public ListNode reverseKGroup(ListNode head, int k) {
      //base case  
      int n = 0;
        ListNode curr = head;
        while(curr != null){
            curr = curr.next;
            n++;
            if (n == k) break;
        }
        if (n<k){
            return head;
        } 
      //reverse the list
        ListNode newhead = null ;
        ListNode temp = head;
        while (temp != curr){
            ListNode newnode = temp.next;
            temp.next = newhead;
            newhead = temp;
            temp = newnode;
        }
        
        head.next = reverseKGroup(curr, k);
        return newhead;
    }
}
```

