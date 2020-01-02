# Backtracking

## Idea
Build solutions incrementally. If at some point of the building the potential solution violate a constraint discard it. This lead to the fact that we find the solutions in a time complexity better than bruteforce

Mainly used to build all permutations, combinations or subsets of a set. This technique allow to rearange in any order of any size a group of elements

Build a tree of call where an argument of the function incrementally build all possible solutions. By adding and deleting the elements to the potential solution before and after the recurive call we can backtrack (rollback the state) in the tree

## Pseudo Code
```
void backtrack(Set incremental, Set not_yet_used) {
  if(incremental is solution)
    solutions.add(incremental)
  else {
    for(element in not_yet_used) {
      incremental.add(element)
      backtrack(incremental, not_yet_used \ {element})
      incremental.delete(element)
    }
  }
}
```

## Cases
### Permutation 
No constraint to satify. Build all possibles orderings. Here backtracking is used for it's exhaustive possiblity testing property

```java
private void permutations(ArrayList<Integer> permutation, Set<Integer> rest) {
        if(rest.isEmpty()) {
            result.add(new ArrayList(permutation));
        } else {
            for(int e : rest) {
                Set<Integer> new_rest = new HashSet(rest);
                permutation.add(e);
                new_rest.remove(e);
                permutations(permutation, new_rest);
                permutation.remove(permutation.size()-1);
            }
        }
    }
    
    public ArrayList<ArrayList<Integer>> permute(ArrayList<Integer> A) {
        result = new ArrayList();
        permutations(new ArrayList<Integer>(), new HashSet(A));
        return result;
    }
```

### Combinations on elements between 1...n of size k
```
If n = 4 and k = 2, a solution is:
[
  [1,2],
  [1,3],
  [1,4],
  [2,3],
  [2,4],
  [3,4],
]
```
Need to disguish beteween {1,2,3} and {1,3,2}. A simple way to not count them twice is to choose only sorted set. Doing this we keep {1,2,3} and discard{1,3,2}. It's backtracking since we do better than bruteforce and are able to cut subtree in the tree of all possibilities during the build of the solution. To do so instead of choosing not_yet_used as set of elements to try, we choose elements to try only if they are greater than the elements of the incremental solution

```java
    private void combinations(List<Integer> combination, int index) {
        if(combination.size() == k){
            solution.add(new ArrayList(combination));
        } else {
            // append to potential solution only element on the right
            for(int num = index; num <= n; ++num) {
                combination.add(num);
                combinations(combination, num+1);
                combination.remove(combination.size()-1);
            }
        }
    }
    
    public ArrayList<ArrayList<Integer>> combine(int A, int B) {
        solution = new ArrayList();
        n = A;
        k = B;
        combinations(new ArrayList<Integer>(), 1);
        return solution;
    }
```
