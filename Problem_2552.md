Titile: 2552. Count Increasing Quadruplets

Given a 0-indexed integer array nums of size n containing all numbers from 1 to n, return the number of increasing quadruplets.

A quadruplet (i, j, k, l) is increasing if:

0 <= i < j < k < l < n, and
nums[i] < nums[k] < nums[j] < nums[l].

Constraints:

4 <= nums.length <= 4000
1 <= nums[i] <= nums.length
All the integers of nums are unique. nums is a permutation.


## Solution:

we can use a nested loop to generate all possible quadruplets (i, j, k, l) satisfying the given conditions, and count the number of increasing quadruplets. The time complexity of this approach is O(n^4),

The question asks us to solve the problem in O(N^2) time. Since we are dealing with quadruplets, we can split them into two pairs, (i, j) and (k, l).

To solve the problem in O(N^2) time, we can use dynamic programming to compute the number of valid pairs for each (i, j) and (k, l). We can use two 2D arrays, pre and post, where pre[i][j] represents the number of valid pairs for (i, j) such that i < j, and post[i][j] represents the number of valid pairs for (k, l) such that k < l. 

In other words, pre[i][j] represents the number of valid pairs that start with index i and pre[i+1][j] will not count in pre[i][j].  

To compute pre[i][j], we can iterate over all pairs of indices (i, j) such that i < j, and count the number of valid pairs. To compute post[i][j], we can iterate over all pairs of indices (k, l) such that k < l, and count the number of valid pairs.

```
for(int i = 1; i<n-1; i++){
    for(int j = 0; j<i;++j){ 
        if(j-1>=0)
            pre[i][j] = pre[i][j-1];  
        if(j-1>=0 && nums[j-1]<nums[i]) 
            pre[i][j]++;     
        }
    }
```

Once we have computed pre and post, we can iterate over all possible values of j and k, and compute the total number of valid quadruplets as the sum of pre[i][j] * post[k][l], where i ranges from 0 to j-1 and l ranges from k+1 to N-1.

```
class Solution {
public:
    long long countQuadruplets(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> pre(n+1,vector<int>(n+1,0));
        vector<vector<int>> suff(n+1,vector<int>(n+1,0));
        for(int i = 0; i<n-1; i++){
            for(int j = n-1; j>i;--j){
                suff[i][j] = suff[i][j+1];
                if(j+1<n && nums[j+1]>nums[i]) suff[i][j]++;
            }
        }
        for(int i = 1; i<n-1; i++){
            for(int j = 0; j<i;++j){
                if(j-1>=0) pre[i][j] = pre[i][j-1];
                if(j-1>=0 && nums[j-1]<nums[i]) pre[i][j]++;
            }
        }
        long long re = 0;
        for(int i = 0; i<n;++i){
            for(int j = i+1; j<n;++j){
                if(nums[i]>nums[j]){
                    re+= (long long)pre[j][i]*suff[i][j];
                }
            }
        }
        return re;
    }
};
```

