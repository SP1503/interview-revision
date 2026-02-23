#revision-1 #revision-2  #revision-3 #revision-4 #written-code-on-18-jan  #CanBeImplementedWithoutRevisit 
## Problem link:
https://www.scaler.com/academy/mentee-dashboard/class/351255/assignment/problems/238?navref=cl_tt_lst_nm
## Understanding:
- Given K linked lists sorted in ascending order
- Return a single linked list sorted in ascending order with all the nodes present in the k lists.
## Input and Output:
![[Screenshot 2025-12-29 at 12.46.06 PM.png]]
## Problem constraints:
![[Screenshot 2025-12-29 at 12.46.25 PM.png]]

## Approach
### Brute Force:
- We know how to sort two sorted list using merge sort algorithm.
- Like the same way we can merge the array one by one.
- **Time complexity:**
	- Sorting two list with size N = 2N
	- Sorting two lists with size 2N and N = 3N
	- Sorting two lists with size 3N and N = 4N
	- In the same way total complexity = 2N + 3N + 4N + ..... + kN
	- O(N (2 + 3 + 4 + 5 + .. K)) = O(N * (K * K - 1) / 2) = O(N * K * K)
	- When N == K then time complexity = O(N * N * N)
- **Space Complexity:**
	- O(1) No space is used

### Optimised Approach:
- We having K lists.
- If we have 2 lists we use two pointers to point to the current node of the lists to find min and merge that.
- In same way we can use K points and compare the values of K pointer and pick smallest and merge it as single linked list.
- For comparing values of K pointers we can use Min Heap.
- **Time Complexity:** 
	- O(M log k) M is the total number of nodes in the k lists and K is the number of lists provided.
- **Space Complexity:**
	- O(K) -> where we insert K pointers in to the heap and maintain it.

### Reference:
![[WhatsApp Image 2025-12-29 at 12.53.48 PM 1.jpeg]]
## Code:

```Java 
private ListNode mergeKLists(ArrayList<ListNode> nodes){
	
	PriorityQueue<ListNode> queue = 
				new PriorityQueue<>(Comparator.comparing(node -> node.val));
	
	for(ListNode node : nodes) queue.add(node);
	
	ListNode head = new ListNode(-1);
	ListNode tail = head;
	
	while(!queue.isEmpty()){
		ListNode min = queue.poll();
		
		tail.next = min;
		tail = tail.next;
		
		if(min.next != null) queue.add(min.next); 
	} 
	
	return head.next;
}
```

