#revision-1 #NeedRevisit 
## Problem link:
- http://scaler.com/academy/mentee-dashboard/class/351256/assignment/problems/35883?navref=cl_tt_lst_nm
## Understanding:
- Given two integer arrays values and weights with N items.
- Given capacity
- Find the maximum value that we can get with given capacity.
- We can break the weight into fractions.
## Input and Output:
![[Screenshot 2026-01-09 at 7.13.01 PM.png]]
![[Screenshot 2026-01-09 at 7.17.49 PM.png]]
## Problem Constraints:
![[Screenshot 2026-01-09 at 7.12.45 PM.png]]
## Approach:
### Brute Force:
- We need to find max happiness that we can get for the given capacity.
- What if we sort the happiness and getting it one by one in descending order.
- What if we get total happiness of a single product with full capacity.
- Other products can give max happiness
- Only way is to find the happiness of each product per unit value.
- Then sort it using descending order and find the total happiness possible.
- Time Complexity: O(N log N)
- Space Complexity: O(N)
### Reference:
### Code
```Java

public class Solution {

class ProductDetail{
	int weight;
	int value;
	double valuePerUnitCost;

	ProductDetail(int weight, int value){
		this.weight = weight;
		this.value = value;
		this.valuePerUnitCost = value * 1.0/(weight);
	}
}

private int findMaxValueForGivenCapacity(
	ArrayList<Integer> values, 
	ArrayList<Integer> weights, 
	int capacity){
	ArrayList<ProductDetail> productDetails = getProductDetails(values, weights);
	
	productDetails.sort((pd1, pd2) -> 
		Double.compare(pd2.valuePerUnitCost, pd1.valuePerUnitCost));
	
	int productIndex = 0;
	double totalValue = 0;
	
	while(capacity > 0 && productIndex < productDetails.size()){
		ProductDetail productDetail = productDetails.get(productIndex);
		
		if(capacity >= productDetail.weight){
			capacity -= productDetail.weight;
			totalValue += productDetail.value;
		}
		else{
			totalValue += 
				(productDetail.value * capacity * 1.0) / productDetail.weight;
			capacity = 0;
		}
		productIndex++;
	}
	return (int) Math.round(totalValue * 100.0 - 0.5);
}

  

private ArrayList<ProductDetail> getProductDetails(
ArrayList<Integer> values, 
ArrayList<Integer> weights){

	ArrayList<ProductDetail> productDetails = new ArrayList<>();
	for(int i = 0; i < values.size(); i++){
		productDetails.add(
			new ProductDetail(weights.get(i), values.get(i)));
	}
	return productDetails;
}

```

