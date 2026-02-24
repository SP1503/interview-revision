## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351233/assignment/problems/40?navref=cl_tt_lst_nm
## Understanding:
- Given: Given a head of a linked list
- To find: Reverse the linked list
## Input and Output:
![[Screenshot 2026-02-03 at 10.21.41 AM.png]]
![[Screenshot 2026-02-03 at 10.21.58 AM.png]]
## Problem Constraints:
![[Screenshot 2026-02-03 at 10.22.30 AM.png]]
## Approach:
### Brute Force:
- Reversing the linked list: The curr node should point to the prev node instead of next node.
- Have a prev variable and mark it as null.
- Iterate the given linked list
	- Save the next of curr node.
	- Mark the current node next to point to prev node.
	- Make curr as prev.
	- Make curr = next;
- Now the head of the linked list will be pointed by prev node. Hence return prev.
- Time Complexity: O(N) At max we will access each node once.
- Space Complexity: O(1) not storing anything.
### Reference:![[WhatsApp Image 2026-02-03 at 10.29.57 AM.jpeg]]
### Code
```Java

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

```

