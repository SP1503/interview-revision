#NeedRevisit #revision-1 #revision-2  #revision-3 #revision-4 #written-code-on-18-jan 
## Problem Link:
https://www.scaler.com/academy/mentee-dashboard/class/351255/assignment/problems/9294?navref=cl_tt_lst_nm

## Understanding:
- Given two array of integer represents the **expiry** and **profit** of product i where i is the index.
- Find the maximum profit we can achieve by selling the n products before its expiry.
## Input and Output:
![[Screenshot 2025-12-29 at 1.09.11 PM.png]]

![[Screenshot 2025-12-29 at 1.09.34 PM.png]]
## Problem constraints:
![[Screenshot 2025-12-29 at 1.09.47 PM.png]]
## Approach:
### Brute Force: Greedy
- Greedy is all about maximising the profit and  minimising the loss.
- How to maximise the profit: Try to sell everything before expiry because all the products have profit. Sort the products based on profit
- Minimise the loss: On sorting based on the expiry we can encounter that a product with high profit can be expired before selling. Hence keep track on the profits that we make while selling
- If we see a product that is having good profit compared to the min of all the profit that we sold then replace that product with current product and add the profit.
- **Time Complexity:**
	- Sorting based on expiry : O(N log N)
	- Iterate the products from i = 0 to i = n
		- add profit in minHeap
		- If current product is not expired sold it
		- if(current product is expired and current product profit is > min of sold prodcut) replace it
		- Add current profit to minHeap
	- total: O(N log N ) + O(N log N) = O(N log N)
- **Space Complexity:**
	- O(N) for storing profits and tracking min of it using heap.

### Reference:

![[WhatsApp Image 2025-12-29 at 1.17.31 PM.jpeg]]
## Code:

```Java 
class ProductInfo{
	int expiry;
	int profit;
	
	ProductInfo(int expiry, int profit){
		this.expiry = expiry;
		this.profit = profit;
	}
}

private int findMaxProfit(ArrayList<Integer> expiry, ArrayList<Integer> profit){
	
	List<ProductInfo> products = new ArrayList<>();
	
	for(int i = 0; i < expiry.size(); i++){
		products.add(new ProductInfo(expiry.get(i), profit.get(i)));
	}
	
	products.sort(Comparator.comparing(prodInfo -> prodInfo.expiry));
	
	PriorityQueue<Integer> profitOrg = new PriorityQueue<>();
	
	int time = 0;
	
	for(ProductInfo productInfo: products){
		if(time <= productInfo.expiry){
			profitOrg.add(productInfo.profit);
			time++;
		} 
		else if(productInfo.profit > profitOrg.peek()){
			profitOrg.poll();
			profitOrg.add(productInfo.profit);
		}
	}
	
	int totalProfit = 0;
	while(!profitOrg.isEmpty()) totalProfit %= profitOrg.poll();
	
	return totalProfit;
}
```




