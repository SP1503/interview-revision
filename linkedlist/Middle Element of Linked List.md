## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351246/assignment/problems/4370?navref=cl_tt_lst_nm
## Understanding:
- Given: 
	- Head of the singly Linked List
- To find:
	- to find the middle element of the linked list.
## Input and Output:
![[Screenshot 2026-02-03 at 1.08.15 PM.png]]
![[Screenshot 2026-02-03 at 1.08.27 PM.png]]
## Problem Constraints:![[Screenshot 2026-02-03 at 1.08.37 PM.png]]
## Approach:
### Brute Force:
- Find the total length of the linked list.
- Find the mid position = total length / 2.
- Now find the list node with mid position.
- **Complexity:**
	- **Time Complexity:** O(N)
	- **Space Complexity:** O(1)
### Optimised Approach:
- We can find the mid element using slow and fast pointers.
- slow = head;
- fast = head;
- At every iteration slow will move to next
- At every iteration fast will move to next.next
- Hence the speed of fast = 2 * speed of slow.
- Hence when the fast reached end, then slow reaches the mid element.
- **Complexity:**
	- **Time Complexity:** O(N)
	- **Space Complexity:** O(1)
### Reference:![[WhatsApp Image 2026-02-03 at 1.21.30 PM.jpeg]]
### Code
```Java

private int findMid(ListNode head){
	ListNode slow = head;
	ListNode fast = head;
	while(fast != null && fast.next != null){
		slow = slow.next;
		fast = fast.next.next;
	}
	return slow.val;
}

```

