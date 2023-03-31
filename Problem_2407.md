Title: 2407. Longest Increasing Subsequence II

TAG: segment tree

1. **Initialize the segment tree**: Create a segment tree with a range from `min(nums)` to `max(nums)` to store the maximum subsequence length for each number in the range.
2. **Process the input array**: Loop through the input array.
3. **Query the segment tree**: For each element, query the segment tree for the longest subsequence length for all valid numbers in the range `[num[i]-k, num[i])`. Let's call the longest subsequence length `max_val`.
4. **Update the segment tree**: Update the segment tree with the new longest subsequence length (i.e., `max_val+1`) for the current element.
5. **Track the maximum subsequence length**: Keep track of the maximum subsequence length found so far.

With this approach, you can find the longest increasing subsequence that meets the requirements in O(n * logM) time complexity, where n is the length of the input array nums, and M is the difference between the maximum and minimum elements in the array.

About Segment tree: https://en.wikipedia.org/wiki/Segment_tree

A segment tree is a data structure used for efficiently performing range-based queries and updates on an array. It is particularly useful when you need to perform multiple queries and updates on the same array. The segment tree is a binary tree in which each node stores information about a specific range of the array. The root node represents the entire array, and the leaf nodes represent individual elements of the array.

In a segment tree, each non-leaf node represents a specific range of the array, which is a combination of its left and right child's ranges. This hierarchical structure allows us to perform operations on a range of elements in O(log n) time, where n is the number of elements in the array.

Some common operations that can be performed using a segment tree include:

    Range Sum Query: Calculate the sum of elements within a given range.

    Range Minimum Query: Find the minimum element within a given range.

    Range Maximum Query: Find the maximum element within a given range.

The segment tree can also be modified to handle other types of range queries or to support different operations on the array, such as addition or multiplication. The key advantage of a segment tree is its ability to perform these operations efficiently and in a relatively short amount of time.

```
template<typename T>
class SegmentTree {
public:
    SegmentTree(const std::vector<T>& data, const std::function<T(T, T)>& op) :
        data(data), n(data.size()), op(op) {
        tree.resize(2 * n);
        identity = 0;
        build();
    }

    T query(int l, int r) {
        T result_left = identity;
        T result_right = identity;
        l += n;
        r += n;
        while (l <= r) {
            if (l % 2 == 1) {
                result_left = op(result_left, tree[l]);
                l++;
            }
            if (r % 2 == 0) {
                result_right = op(tree[r], result_right);
                r--;
            }
            l /= 2;
            r /= 2;
        }
        return op(result_left, result_right);
    }

    void update(int idx, T value) {
        idx += n;
        tree[idx] = value;
        for (idx /= 2; idx >= 1; idx /= 2) {
            tree[idx] = op(tree[2 * idx], tree[2 * idx + 1]);
        }
    }

private:
    const std::vector<T>& data;
    int n;
    std::vector<T> tree;
    std::function<T(T, T)> op;
    T identity;

    void build() {
        for (int i = 0; i < n; ++i) {
            tree[n + i] = data[i];
        }
        for (int i = n - 1; i > 0; --i) {
            tree[i] = op(tree[2 * i], tree[2 * i + 1]);
        }
    }
};

class Solution {
public:
    int lengthOfLIS(vector<int>& nums, int k) {
        vector<int> a(100001);
        SegmentTree<int> m(a, [](int a, int b) { return std::max(a, b); });

        for (int i = 0; i < nums.size(); i++)
        {
            int a = nums[i];
            int l = a - k; 
            if (l < 0) l = 0;

            int max = m.query(l, a-1);
            m.update(a, max + 1);
        }

        return m.query(0, 100000);
    }
};

```

Similar easy version is: 
https://leetcode.com/problems/longest-increasing-subsequence/

There are nice greedy method for above problem: https://www.geeksforgeeks.org/longest-monotonically-increasing-subsequence-size-n-log-n/

But not common as segment tree method.
