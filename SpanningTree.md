# spanning tree

## UnionFind or Disjoint Sets
### Principle
The point of the structure is to provide a fast way to test if two elements belongs to a same group and to quickly union these groups

Elements are index of an array to be accessed in constant time. The value at their index correspond to their group. Initially all elements are in the same group and the array support the property arr[i] = i

Structure has to support find(a, b) to check if two elements are in the same set. It also support union(a,b) to merge sets containing a and b together.

### Naive apporche: find --> O(1) union --> O(N)
Go through all the array each time two sets are merged to put the same value for all the set --> Take M*N for M unions

find is just arr[a] == arr[b] --> O(1)

### Efficient Approache find --> ln(N) union --> ln(n)
Each set has a root node with arr[arr[...arr[i]...]] = root. So to check if two elements are from the same set, we can compare if they have the same root.

To do an union we can just make value of root of one set to be root of the other set---> Problem: this can lead to very tall tree and we and up with O(N) complexity

To flatten the structure we maintain the size of each set and merge each timt the smaller set in the bigger. And durring the phase of root computation we make each node seen pointing to his grand parent to compress the deepness. We have to make the bigger set become the overall root to minimize the number of changes to do during the union phase

#### Implementation
```java
public class UnionFind {
    private int sets [];
    //maintain size of the sets
    private int sizes [];

    public UnionFind(int size) {
        sets = new int[size];
        sizes = new int [size];
        for (int i = 0; i < size; i++) {
            sets[i] = i;
            sizes[i] = 1;
        }
    }

    private int root(int e) {
        while(sets[e] != e) {
            // make node to point to his grand parent to flatten the tree
            sets[e] = sets[sets[e]];
            e = sets[e];
        }

        return e;
    }

    // Log(N)
    public boolean find(int a, int b) {
        return root(a) == root(b);
    }

    // Log(N)
    public void union(int a, int b) {
        int i = root(a);
        int j = root(b);

        if(sizes[i] <= sizes[j]) {
            sets[i] = j;
            sizes[j] += sizes[i];
        } else {
            sets[j] = i;
            sizes[i] += sizes[j];
        }
    }

    public static void main(String[] args) {
       UnionFind u =  new UnionFind(10);
       u.union(3, 4);
       u.union(4, 9);
       u.union(8, 0);
       u.union(3, 2);
       u.union(5, 6);
       u.union(5,9);
       u.union(7,3);
       u.union(4,8);
       u.union(6,1);
    }
```

### Minimum Spanning Tree
Tree = connected acyclique graph
Minimum Spanning Tree = subset of a graph forming a tree containing all vertices of the graph with overall minimal cost.
Minimal weight connected subgraph containing all vertices

#### Kruskal
Greedy approach works fine. sort the edges by weight. Add edges from smaller weight to bigger if it doesn't form a cycle until to have V-1 edges. Use UnionFind structure to check for cycles in Ln complexity time . Don't add an edge with two ends in the same set to avoid cycles.

```java
public int kruskal(int A, ArrayList<ArrayList<Integer>> B) {
        init(A);
        Collections.sort(B, Comparator.comparingInt(e -> e.get(2)));
        Set<Integer> tree = new HashSet();
        int cost = 0;
        int nb_edge = 0;
        
        for(List<Integer> edge: B) {
            if(nb_edge == A-1) {
                break;
            }
            
            Integer node1 = edge.get(0)-1;
            Integer node2 = edge.get(1)-1;
            if(!find(node1, node2)){
                cost += edge.get(2);
                nb_edge++;
                union(node1, node2);
            }
        }
        
        return cost;
    }
```
