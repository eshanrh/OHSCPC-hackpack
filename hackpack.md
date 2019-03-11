# Competitive Programming Hackpack
## Oviedo High School

### Binary Index Tree
```java
class bit {

	public long[] cumfreq;

	// Do indexes 1 to n.
	public bit(int n) {

		int size = 1;
		while (size < q) size <<= 1;
		n = size;

		cumfreq = new long[n+1];
	}

	// Uses 1 based indexing.
	public void add(int index, long value) {
		while (index < cumfreq.length) {
			cumfreq[index] += value;
			index += Integer.lowestOneBit(index);
		}
	}

	// Returns the sum of everything upto index.
	public long sum(int index) {
		long ans = 0;
		while (index > 0) {
			ans += cumfreq[index];
			index -= (Integer.lowestOneBit(index));
		}
		return ans;
	}

	// Use 1 based indexing.
	public long sum(int low, int high) {
		return sum(high) - sum(low-1);
	}

	// Return the total number of items in the BIT.
	public long all() {
		return sum(cumfreq.length-1);
	}

	// Return the total number of items in the BIT at or above index.
	public long atOrAbove(int index) {
		long sub = 0;
		if (index > 0) sub = sum(index-1);
		return all() - sub;
	}
}
```

### DFS
graph is G
```java
void DFS(int at, boolean[] V)
{
  System.out.printf("At node %d in the DFS%n",at);

  // mark that we are visiting this node
  V[at]=true;

  // recursively visit every node connected yet to be visited
  for (int i=0; i<N; ++i)
    if (G[at][i] && !V[i])
    {
      System.out.printf("Going to node %d...",i);
      DFS(i,V);
    }
  System.out.printf("Done processing node %d%n", at);
}
```

### Edge class
```java
static class Edge implements Comparable<Edge> {
  int e, w;

  public Edge(int e, int w) {
    this.e = e;
    this.w = w;
  }

  public int compareTo(Edge o) {
    return w - o.w;
  }
}
```

### Dijkstra
```java
static int oo = (int) 1e9;
static int n;
static ArrayList<Edge>[] g;

// Returns the shortest distance from vertex s to d.
public static int dijkstras(int s, int d) {

  // Set up the priority queue.
  boolean[] visited = new boolean[n];
  PriorityQueue<Edge> pq = new PriorityQueue<Edge>();
  pq.add(new Edge(s, 0));

  // Go till empty.
  while (!pq.isEmpty()) {

    // Get the next edge.
    Edge at = pq.poll();
    if (visited[at.e]) continue;
    visited[at.e] = true;

    // We made it, return the distance.
    if (at.e == d) return at.w;

    // Enqueue all the neighboring edges.
    for (Edge adj : g[at.e])
      if (!visited[adj.e]) pq.add(new Edge(adj.e, adj.w + at.w));
  }
  return oo;
}
```

### Knapsack
```java
// Returns an array of size maxW+1, such that array[i] stores the max value
// of a knapsack with weight at most i. Item i has weight weights[i] and
// value values[i].
public static int[] solveKnapsack(int[] weights, int[] values, int maxW) {

  int[] dp = new int[maxW+1];

  // Loop through items.
  for (int i=0; i<weights.length; i++)

    // Go backwards through possible weights to prevent using this item
    // more than once.
    for (int j=maxW; j>=weights[i]; j--)
      dp[j] = Math.max(dp[j], dp[j-weights[i]]+values[i]);

  // Ta da!
  return dp;
}
```

### Disjoint-Set
```java
// Just stores a pair of values associated with an item in a disjoint set.
class Pair {
	
	public int value;
	public int height;
	
	public Pair(int v, int h) {
		value = v;
		height = h;
	}
}

public class DisjointSet {
	
	// This array stores our data structure.
	private Pair[] parentList;
	
	// Initialize each item to a value from 0 to n-1 all with a height of 0, meaning separate trees.
	public DisjointSet(int n) {
		parentList = new Pair[n];
		for (int i=0; i<n; i++) {
			parentList[i] = new Pair(i,0);
		}
	}
	
	// Returns the value that is the root of the tree that stores value.
	public int find(int value) {
		
		// Continue going up the ancestral tree until you get to a node without a parent.
		while (parentList[value].value != value) {
			value = parentList[value].value;
		}
		
		// Return this root.
		return value;
	}
	
	// Create a union between the tree storing indexA and the one storing indexB.
	// Precondition: the values indexA and indexB are stored in different subtrees.
	public void union(int indexA, int indexB) {
		
		// Get each respective root node.
		int rootA = find(indexA);
		int rootB = find(indexB);
		
		// Attach tree A to be a child of tree B since A is shorter.
		if (parentList[rootA].height < parentList[rootB].height) {
			parentList[rootA].value = rootB;
		}
		else {
			
			// Attach B to be the child of A.
			parentList[rootB].value = rootA;
			
			// If the trees are equal height, then A's root goes up 1 in height.
			if (parentList[rootA].height == parentList[rootB].height)
				parentList[rootA].height++;
		}
	}
	
	// For debugging purposes. Prints out the parent and height of each node.
	public void print() {
		for (int i=0; i<parentList.length; i++) {
			System.out.print(parentList[i].value+"\t");
		}
		System.out.println();
		
		for (int i=0; i<parentList.length; i++) {
			System.out.print(parentList[i].height+"\t");
		}
		System.out.println("\n");
	}
```

