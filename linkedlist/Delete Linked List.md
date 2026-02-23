## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351233/assignment/problems/30667?navref=cl_tt_lst_nm
## Understanding:
- Given: 
	- Head of a linked List
	- Position of a node to remove
- Delete the given position in the linked list
- To find:
	- Return the updated list
## Input and Output:![[Screenshot 2026-02-03 at 12.36.52 PM.png]]
![[Screenshot 2026-02-03 at 12.37.03 PM.png]]
## Problem Constraints:![[Screenshot 2026-02-03 at 12.37.15 PM.png]]
## Approach:
### Brute Force:
- If the given pos is 0 then return the next of head.
- If the given  linked list is empty return null
- Iterate the given linked list
	- find the prev node of given pos
	- if prev == null return head
	- store the next node
	- Check if next != null, If yes, point the next of current to the next of next.
	- Return the head.
- **Complexity**:
	- **Time Complexity:** O(N)
	- **Space Complexity:** O(1)
### Reference:![[WhatsApp Image 2026-02-03 at 12.57.18 PM.jpeg]]
### Code
```Java

private ListNode delete(ListNode A, int B){
	if(A == null) return null;
	else if(B == 0) return A.next;
	else{
	int pos = 0;
	ListNode curr = A;
	while(pos < B - 1){
			pos++;
			curr = curr.next;
		}
		if(curr == null) return A;
		else{
			ListNode next = curr.next;
			if(next != null) curr.next = next.next;
			return A;
		}
	}
}
```

