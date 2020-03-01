# Dijkstra with min Priority Queue V + Elog(v)

Greedy algorithm relying on BFS to compute single source shortest paths. Use a structure to store a mapping of vertices with the minimal cost to get to them in increasing cost order. The gready component of the algorithm is based on the fact that the minimal cost mapping in the structure contain the shortest to this vertices at each iterations

## Pseudocode

```java
void relax(previousNode, node) {
  currentCost = shortestPaths.get(node);
  newCost = shortestsPaths.get(previousNode) + previousNode.edge[node]
  if(currentCost < newCost) {
    verticesQueue.add(node);
    shortestPaths.put(node, newCost)
  }
}

// At the end mapping contain shortest path from src to all vertices
void dijkstra(Node src, Node dst) {
  vertices = new Queue();
  shortestPaths = new HashMap();
  vertices.add(src);
  shortesPaths.add(src, 0);

  while(found contains all vertices) {
    node = vertices.poll();
    found.add(node);
    
    // Use this in case of update cost of node in queue is avoided
    // Queue contains nodes copy of the same node with different cost
    // Use only the one with minimal cost and discard others
    if(node in found)
      continue

    for(neigh in node.neighboors){
      //update shortestPaths of a node
      // if node doesn't exist in the map assume previous cost is infinity
      relax(neigh, shortestPaths.get(node)+node.edge[neigh])
    }
  }
}
```

## Implementation for Knight On Chess Board problem

```java

public class KnightOnChessBoard {
    // get all possibles move for a knight on a board
    private int X [] = new int [] { 1, 2, 2, 1, -1, -2, -2, -1};
    private int Y [] = new int [] { -2, -1, 1, 2, 2, 1, -1, -2};
    private PriorityQueue<Node> vertices;
    private HashMap<Node, Integer> shortests;
    private Set<Node> found;
    
    private int A;
    private int B;

    public class Node {
        int x;
        int y;
        int cost;

        Node(int x, int y) {
            this.x = x;
            this.y = y;
        }

        Node(int x, int y, int cost) {
            this.x = x;
            this.y = y;
            this.cost = cost;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Node node = (Node) o;
            return x == node.x &&
                    y == node.y;
        }

        @Override
        public int hashCode() {
            return Objects.hash(x, y);
        }
    }

    private void relax(Node node, int new_cost) {
        Integer prev_cost = shortests.get(node);
        if(prev_cost == null || prev_cost > new_cost) {
            node.cost = new_cost;
            vertices.add(node);
            shortests.put(node, new_cost);
        }
    }

    private void processNeighboors(Node n) {
        for(int i = 0; i < 8; ++i) {
            int x = n.x + X[i];
            int y = n.y + Y[i];

            if(x < 1 || y < 1 || x > A || y > B || found.contains(new Node(x,y)))
                continue;

            Node neighboor = new Node(x, y);
            relax(neighboor, n.cost+1);
        }
    }

    /*  A: number of lines in the board
        B: number of columns
        C,D: initial position
        E,F: final position
    */
    public int knight(int A, int B, int C, int D, int E, int F) {
        vertices = new PriorityQueue<>(Comparator.comparingInt(e -> e.cost));
        shortests = new HashMap<>();

        found = new HashSet<>();
        vertices.add(new Node(C, D, 0));
        shortests.put(new Node(C, D), 0);

        this.A = A;
        this.B = B;

        //O(V log V + E log(V))
        while(found.size() <= A*B) {
            //O(log V)
            Node n = vertices.poll();

            if(n == null)
                break;

            // O(1)
            if(found.contains(n))
                continue;

            // dst
            if(n.x == E && n.y == F) {
                return n.cost;
            }


            found.add(n);
            //O(V) in case of complete graph
            processNeighboors(n);
        }

        return -1;
    }

```
  