## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351246/assignment/problems/36/submissions
## Understanding:
- Given:
	- head1 and head2 represents the head of two linked lists sorted in ascending order.
- To Return:
	- Return the merged form of two lists in sorted order.
## Input and Output:![[Screenshot 2026-02-03 at 8.19.59 PM.png]]
![[Screenshot 2026-02-03 at 8.20.08 PM.png]]
## Problem Constraints:
![[Screenshot 2026-02-03 at 8.20.43 PM.png]]
## Approach:
### Brute Force:
- Merge two linked lists.
- Sort the total linked list (We can do merge sort)
- Complexity:
	- Time Complexity: O(N + N log N + N)
	- Space Complexity: O(1)
### Optimised Approach:
- Create two pointers pointing to head1 and head2.
- Iterate until any two nodes become null
	- Check if p1.val <= p2.val
		- If yes connect the node
		- Mark the current node as tail
		- Move to next node of p1.
	- Else
		- Connect the p2 node.
		- Make the current node as tail node.
		- Move to next node of p2.
- Check if p1 != null
	- If yes connect the node
	- Mark the current node as tail
	- Move to next node of p1.
- Check if p2 != null
	- Connect the p2 node.
	- Make the current node as tail node.
	- Move to next node of p2.
- return the head.next
### Reference:![[WhatsApp Image 2026-02-03 at 8.33.07 PM.jpeg]]
### Code
```Java

private ListNode merge(ListNode head1, ListNode head2){
	ListNode head = new ListNode(-1);
	ListNode tail = head;
	ListNode p1 = head1;
	ListNode p2 = head2;
	while(p1 != null && p2 != null){
		if(p1.val <= p2.val){
			tail.next = p1;
			tail = p1;
			p1 = p1.next;
		}
		else{
			tail.next = p2;
			tail = p2;
			p2 = p2.next;
		}
	}
	
	while(p1 != null){
		tail.next = p1;
		tail = p1;
		p1 = p1.next;
	}
	while(p2 != null){
		tail.next = p2;
		tail = p2;
		p2 = p2.next;
	}
	return head.next;
}

```

