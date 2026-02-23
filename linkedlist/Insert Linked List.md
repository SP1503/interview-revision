## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351233/assignment/problems/30536/submissions
## Understanding:
- Given:
	- Head of the singly linked list.
	- Value to insert
	- Position to insert
- To find:
	- Update the linked list with inserted node.
## Input and Output:
![[Screenshot 2026-02-03 at 11.36.47 AM.png]]
![[Screenshot 2026-02-03 at 11.37.08 AM.png]]
## Problem Constraints:![[Screenshot 2026-02-03 at 11.37.19 AM.png]]
## Approach:
### Brute Force:
- At any point of time we need to insert a new node into the given linked list.
- Hence creating the new node.
- If the given pos is 0 then the new node is hrad.
- If the given linked list is null then this is the only node that will be present.
- Iterate the given linked list
	- Find the current position.
	- If the pos == requested pos - 1
	- then this is the node that needs to point to new node.
	- newNode.next = curr.next;
	- curr.next = newNode
	- Position ++
	- curr = curr.next;
- if the pos is greater than length of the given linked list then attach the node at the end.
- return head.
- **Complexity:**
	- **Time Complexity:** O(N)
	- **Space Complexity:** O(1)
### Reference:
### Code
```Java

private ListNode insert(ListNode A, int B, int C){
	ListNode newNode = new ListNode(B);
	if(C == 0){
		newNode.next = A;
		return newNode;
	}
	else if(A == null) return newNode;
	else{
		int pos = 0;
		ListNode prev = null;
		ListNode curr = A;
		while(curr != null){
			if(C - 1 == pos){
				ListNode next = curr.next;
				curr.next = newNode;
				newNode.next = next;
				return A;
			}
			prev = curr;
			curr = curr.next;
			pos++;
		}
		prev.next = newNode;
		return A;
	}
}

```


