## Problem link:
## Understanding:
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:![[WhatsApp Image 2026-02-20 at 6.55.35 PM.jpeg]]
### Code
```Java
// Brute Force
// Time Compelxity: O(N * N * N)
// Space Complexity: O(1)
private int findSubWithSumZero(int[] ele){
	
	int len = ele.length;
	for(int i = 0; i < len; i++){
		for(int j = i; j < len; j++){
			int sum = 0;
			for(int k = i; k <= j; k++){
				sum += ele[k];
			}
			if(sum == 0) return 1;	
		}
	}
	
	return 0;
}

// Time Compelxity: O(N * N)
// Space Complexity: O(1)
// Using carry forward
private int findSubWithSumZero(int[] ele){
	
	int len = ele.length;
	for(int i = 0; i < len; i++){
		int sum = 0;
		for(int j = i; j < len; j++){
			// Carryforwarding the sum fior every subarray start with index i 
			sum += ele[j];
			if(sum == 0) return 1;	
		}
	}
	
	return 0;
}

// Optimised
// Need to find subarray with sum = 0
// prefixSum[j] - prefixSum[i - 1] == 0
// PrefixSum[i] == 0
// Time Complexity: O(N)
// Space Complexity: O(N)
private int findSubWithSumZero(int[] ele){
	
	int len = ele.length;
	int[] prefix = new int[len];
	
	prefix[0] = ele[0];
	
	for(int i = 1; i < len; i++) prefix[i] = prefix[i - 1] + ele[i];
	
	Set<Integer> hash = new HashSet<>();
	
	for(int i = 0; i < len; i++){
		if(prefix[i] == 0) return 1;
		else if(hash.contains(prefix[i])) return 1;
		else hash.add(prefix[i]);
	}
	
	return 0;
}

// Optimised
// Need to find subarray with sum = 0
// prefixSum[j] - prefixSum[i - 1] == k
// PrefixSum[i] == k
// Time Complexity: O(N)
// Space Complexity: O(N)
private int findSubWithSumZero(int[] ele){
	
	int len = ele.length;
	int[] prefix = new int[len];
	
	prefix[0] = ele[0];
	
	for(int i = 1; i < len; i++) prefix[i] = prefix[i - 1] + ele[i];
	
	Map<Integer, Integer> hash = new HashMap<>();
	int count = 0;
	for(int i = 0; i < len; i++){
		if(prefix[i] == k) return count++;;
		else if(hash.containsKey(prefix[i] - k)) 
			return count += hash.get(prefix[i]- k);
		else hash.put(prefix[i], hash.getOrDefault(prefix[i], 0) + 1);
	}
	
	return 0;
} 
```


