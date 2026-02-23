## Problem link:
- https://leetcode.com/discuss/post/1031933/amazon-onsite-sde2-package-dependencies-tuw9e/
## Understanding:
- a) You have a package repository in which there are dependencies between packages for building like package A has to be built before package B. If you are given dependencies between the packages and package name x, we have find the build order for x.  
Ex: A → {B,C}  
B → {E}  
C → {D,E,F}  
D → {}  
F → {}  
G → {C}

For package A, build order is E B F D C A (may not unique)

Given a function Set getDependencies (Package packageName) which returns a set of dependencies for a given package name, write a method List getBuildOrder(Package packageName) which returns the build order

b) How would you handle cyclic dependencies (Algo only)

reference: https://leetcode.com/problems/course-schedule-ii/
## Input and Output:
## Problem Constraints:
## Approach:
### Brute Force:
### Optimised Approach:
### Reference:
- a) Solution using DFS, Top Sort, and adding to result at the node with no dependencies recursively.  
- b) Can add check for cycles inside Top Sort recursive function.

```Java

private List<Character> resolOrder = new ArrayList<>();

private boolean resolveDependencies(
Map<Character, List<Character>> depMap, 
Character currDep, 
Set<Character> resolvPath, 
Set<Character> installed){

	// Base Condition
	if(resolvPath.contain(currDep)) return false;
	else if(installed.contains(currDep)) return true;
	else{
		// Recurrence Relation
		resolvPath.add(currDep);
		for(Character dep : depMap.getOrDefault(currDep, new ArrayList<>())){
			// Forms a cycle, Hence returning false
			if(resolvPath.contains(dep)) return false;
			
			// Resolving only unresolved packages
			if(!installed.contains(dep)){
				if(!resolveDependencies(depMap, dep, resolvPath, installed)){
					return false;
				}
			}
		}
		
		resolOrder.add(currDep);
			
		installed.add(currDep);
		resolvPath.remove(currDep);
		
		return true;
	}
}

Time Compelxity: O(V + E)
Space Complexity: O(V + E)
```


