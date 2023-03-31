Title 1483. Kth Ancestor of a Tree Node,  using C++

In the LeetCode problem 1483 "Kth Ancestor of a Tree Node," you are given a tree consisting of n nodes numbered from 0 to n-1, with an array parent[] where parent[i] is the parent of ith node. The tree node 0 is the tree's root, and each node has at most one parent.

You need to implement the class TreeAncestor, which contains the following methods:

TreeAncestor(int n, vector<int>& parent) - Initializes the object with the number of nodes in the tree and the parent array.

int getKthAncestor(int node, int k) - Returns the kth ancestor of the given node. If there is no such ancestor, return -1.

Here's a possible C++ solution:

```
class TreeAncestor {
public:
    vector<vector<int>> dp;

    TreeAncestor(int n, vector<int>& parent) {
        int maxDepth = log2(n) + 1;
        dp = vector<vector<int>>(n, vector<int>(maxDepth, -1));

        for (int i = 0; i < n; ++i) {
            dp[i][0] = parent[i];
        }

        for (int j = 1; j < maxDepth; ++j) {
            for (int i = 0; i < n; ++i) {
                if (dp[i][j - 1] != -1) {
                    dp[i][j] = dp[dp[i][j - 1]][j - 1];
                }
            }
        }
    }

    int getKthAncestor(int node, int k) {
        int maxDepth = dp[0].size();
        for (int j = maxDepth - 1; j >= 0; --j) {
            if (k & (1 << j)) {
                node = dp[node][j];
                if (node == -1) {
                    return -1;
                }
            }
        }
        return node;
    }
};
```

This solution uses a dynamic programming table dp[][] to store the ancestors of all nodes at different depths. The getKthAncestor() function has a time complexity of O(log(n)).

The dynamic programming (DP) table dp[i][j] stores the 2^j-th ancestor for node i. The table is built using a bottom-up approach, and it has dimensions n x log2(n) + 1, where n is the number of nodes in the tree.

Building the DP table:

For each node i (0 to n-1), set dp[i][0] as the parent of node i. This is the base case where the first ancestor (2^0-th ancestor) of each node is their immediate parent.

Iterate through the powers of two, starting from 1 (2^j). For each power of two, compute the ancestor for each node i (0 to n-1) as follows:

a. If the (2^(j-1))-th ancestor of node i is not -1 (meaning it exists), then the (2^j)-th ancestor of node i is the (2^(j-1))-th ancestor of its (2^(j-1))-th ancestor. This is derived from the property that the 2^j-th ancestor of a node is the 2^(j-1)-th ancestor of its 2^(j-1)-th ancestor.

b. Set dp[i][j] to the computed (2^j)-th ancestor.

Searching for the k-th ancestor:

To find the k-th ancestor of a given node, we utilize the binary representation of k. The binary representation of k helps us break down k into the sum of powers of two.

Iterate through the powers of two (0 to log2(n)), and for each power of two (2^j), check if the j-th bit in the binary representation of k is set (1). This can be done using the bitwise AND operation: k & (1 << j).

If the j-th bit is set, that means the (2^j)-th ancestor contributes to finding the k-th ancestor. Update the node by moving to its (2^j)-th ancestor: node = dp[node][j]. If the node becomes -1, that means there is no k-th ancestor, so return -1.

After iterating through all powers of two, the final value of node is the k-th ancestor of the given node.

By using the DP table and binary representation of k, we can find the k-th ancestor in O(log(n)) time.

Let's use the following sample data to illustrate how the DP table is built

```
n = 7
parent = {-1, 0, 0, 1, 1, 2, 2}
```

The tree structure would look like this:
```
    0
   / \
  1   2
 / \ / \
3  4 5  6
```

Now, let's build the DP table. The maximum depth of the DP table would be log2(7) + 1 = 3.

First, initialize the DP table with dimensions 7x3, filled with -1:

```
dp = [    [-1, -1, -1],
    [-1, -1, -1],
    [-1, -1, -1],
    [-1, -1, -1],
    [-1, -1, -1],
    [-1, -1, -1],
    [-1, -1, -1]
]
```

Set the first column (2^0-th ancestors) to be the immediate parents of each node:

```
dp = [    [-1, -1, -1],
    [ 0, -1, -1],
    [ 0, -1, -1],
    [ 1, -1, -1],
    [ 1, -1, -1],
    [ 2, -1, -1],
    [ 2, -1, -1]
]
```
Now, compute the 2^1-th ancestors for each node:
```
dp = [    [-1, -1, -1],
    [ 0,  0, -1],
    [ 0,  0, -1],
    [ 1,  0, -1],
    [ 1,  0, -1],
    [ 2,  0, -1],
    [ 2,  0, -1]
]
```

Finally, compute the 2^2-th ancestors for each node:

```
dp = [    [-1, -1, -1],
    [ 0,  0, -1],
    [ 0,  0, -1],
    [ 1,  0,  0],
    [ 1,  0,  0],
    [ 2,  0,  0],
    [ 2,  0,  0]
]
```
Now, we can use this DP table to find the k-th ancestor of any given node using the binary representation of k as explained in the previous answer.

