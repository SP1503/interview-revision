## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351246/assignment/problems/331/?navref=cl_pb_nv_tb
## Understanding:
- Given:
	- Head of the singly linked list
- To find: 
	- Find whether the given linked list is a palindrome
## Input and Output:
![[Screenshot 2026-02-03 at 1.58.42 PM.png]]
![[Screenshot 2026-02-03 at 1.58.54 PM.png]]
## Problem Constraints:
![[Screenshot 2026-02-03 at 1.59.05 PM.png]]

## Approach:
### Brute Force:
- Find the middle element of the Linked list.
- Partition the linked list in the middle.
- Reverse the second part
- Check if both the lists are equal
- Complexity:
	- Time Complexity: O(N)
	  Space Complexity: O(1)
### Reference:![[WhatsApp Image 2026-02-03 at 2.05.42 PM.jpeg]]
### Code
```Java

private boolean isPalin(ListNode head1){
	ListNode mid = findMid(head1);
	ListNode head2 = reverse(mid.next);
	mid.next = null;
	ListNode p1 = head1;
	ListNode p2 = head2;
	while(p1 != null && p2 != null){
		if(p1.val != p2.val) return false;
		p1 = p1.next;
		p2 = p2.next;
	}
	return true;
}

  

private ListNode reverse(ListNode head){
	ListNode prev = null;
	ListNode curr = head;
	while(curr != null){
		ListNode next = curr.next;
		curr.next = prev;
		prev = curr;
		curr = next;
	}
	return prev;
}

  

private ListNode findMid(ListNode head){
	ListNode slow = head;
	ListNode fast = head.next;
	while(fast != null && fast.next != null){
		slow = slow.next;
		fast = fast.next.next;
	}
	return slow;
}
```

