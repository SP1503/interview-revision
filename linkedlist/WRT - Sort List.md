## Problem link:
- https://www.scaler.com/academy/mentee-dashboard/class/351246/assignment/problems/34/?navref=cl_pb_nv_tb
## Understanding:
- Given:
	- Head of the singly Linked list
- To return:
	- Return the linked list in sorted order.
## Input and Output:![[Screenshot 2026-02-03 at 8.39.54 PM.png]]![[Screenshot 2026-02-03 at 8.40.23 PM.png]]
![[Screenshot 2026-02-03 at 8.40.13 PM.png]]
## Problem Constraints:![[Screenshot 2026-02-03 at 8.40.52 PM.png]]
## Approach:
### Brute Force:
- Check if the current linked list is null if yes return null
- Check if current linked list if the only node, if yes return node.
- For the linked list with length greater than 2 we can proceed with recurrence relation
	- Find the mid node of the linked list
	- partition the given linked list into two by making mid.next = null
	- Sort the partition 1 and partition 2 recursively.
	- Merge both the parts.
	- return the merged list
- **Complexity**:
	- **Time Complexity**: O(N ) * O(log N) at every level we iterate N nodes and total number of levels is Log N.
	- **Space Complexity**: O(log N)
### Reference:
### Code
```Java

private ListNode mergeSort(ListNode head){
	if(head == null) return head;
	else if(head.next == null) return head;
	else{
		ListNode mid = findMid(head);
		ListNode p1 = head;
		ListNode p2 = mid.next;
		mid.next = null;
		p1 = mergeSort(p1);
		p2 = mergeSort(p2);
		return merge(p1, p2);
	}
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

