Title: problem 2338 ou are given two integers n and maxValue, which are used to describe an ideal array.

TAG: MATH DP

A 0-indexed integer array arr of length n is considered ideal if the following conditions hold:

Every arr[i] is a value from 1 to maxValue, for 0 <= i < n.
Every arr[i] is divisible by arr[i - 1], for 0 < i < n.
Return the number of distinct ideal arrays of length n. Since the answer may be very large, return it modulo 109 + 7.

Solution 1 O(n * maxValue^2)

The base case is dp[1][j] = 1 for all j from 1 to maxValue, since there is only one ideal array of length 1.

For i > 1, we can calculate dp[i][j] as follows:

dp[i][j] = sum(dp[i-1][k]) for all k such that k divides j.

Finally, the answer to the problem is the sum of dp[n][j] for all j from 1 to maxValue.

```
for (int i = 2; i <= n; i++) {
        for (int j = 1; j <= maxValue; j++) {
            for (int k = j; k <= maxValue; k += j) {
                dp[i][j] += dp[i-1][k];
                dp[i][j] %= MOD;
            }
        }
    }

```

Solution 2:

Consider a strict increasing sequence such as 1, 3, 5. Let's examine the case where N=5. In this case, we can have sequences like 11135, 11335, 13335, and 13555. To create such sequences, we can think of dividing the 5 positions into 3 groups. The number of ways to do this can be calculated using the combination formula: C(N-1, K-1), where K represents the number of items in the sequence. In this scenario, we need to choose K-1 "bar" positions from the N-1 available positions.

```
_ | _ | _ _ _ (this translates to 1 | 3 | 5 5 5 )
_ _ | _ | _ _ (this translates to 1 1 | 3 | 5 5 )
```

The next step is to determine how to find possible increasing sequences. Since each item in the sequence must be a divisor of the previous item, we can choose the smallest factor, which is 2. Note that 2^14 > 10,000, so the longest strictly increasing ideal array has 14 elements.

To caculate  C(N-1, K-1), we need build Pascal's Triangle.

create a program to generate the first N rows of Pascal's Triangle, which will include all binomial coefficients for N choose K, where 0 <= K <= N:

```
std::vector<std::vector<long long>> generate_pascals_triangle(int n) {
    std::vector<std::vector<long long>> pascals_triangle(n);

    for (int i = 0; i < n; ++i) {
        pascals_triangle[i].resize(i + 1);
        pascals_triangle[i][0] = 1;
        pascals_triangle[i][i] = 1;

        for (int j = 1; j < i; ++j) {
            pascals_triangle[i][j] = pascals_triangle[i - 1][j - 1] + pascals_triangle[i - 1][j];
        }
    }

    return pascals_triangle;
}
```

Because Max of K is 14, so just need build partital pascals_triangle

```
for(int i=0; i<=n; i++) {
    //C[i].resize(19);
	C[i][0]=1;
	for(int j=1; j<=min(i,18); j++) {
		C[i][j]=(C[i-1][j]+C[i-1][j-1])%MOD;
	}
}
```

Next step need found how many possible ideal sequence with difference item with items number is k, and max value is maxValue

It is typical dp problem. dp[k][j]  k is how many items; j is max number. 

|j\K | 1|
|--|--|
|1 | 1|
|2 | 1|
|3 | 1|
|4 | 1|
|5 | 1|
|6 | 1|
|7 | 1|
|8 | 1|
|9 | 1|

Try to build lay 2.  from 1 of lay 1.  Possible max value is 2 3 4 5,6, 7, 8, 9,should + dp[2][1]

|j\K | 1| 2 |
|--|--|--|
|1 | 1| |
|**2** | 1| 1|
|**3** | 1| 1 |
|**4** | 1| 1|
|**5** | 1|  1|
|**6** | 1|  1|
|**7**| 1|  1|
|**8** | 1| 1|
|**9** | 1| 1 |

from 2 of lay 1.  Possible max value is 4 6 8 should + dp[2][1]

|j\K | 1| 2 |
|--|--|--|
|1 | 1|  1|
|2 | 1| 1|
|3 | 1|  1|
|**4** | 1| **2**|
|5 | 1|  1|
|**6** | 1|  **2**|
|7 | 1|  1|
|**8** | 1| **2**|
|9 | 1|  1|

from 3 of lay 1, possible max value is 6, 9, should + dp[3][1]

|j\K | 1| 2 |
|--|--|--|
|1 | 1|  |
|2 | 1| 1|
|3 | 1| 1 |
|4 | 1| 2|
|5 | 1|  1|
|**6** | 1| **2** |
|7 | 1|  1|
|8 | 1| 2|
|**9** | 1| **2** |

from 4 of lay 1,, possible max value is 8, should + dp[4][1]

|j\K | 1| 2 |
|--|--|--|
|1 | 1|  |
|2 | 1| 1|
|3 | 1| 1 |
|4 | 1| 2|
|5 | 1| 1|
|6 | 1| 2|
|7 | 1| 1|
|**8** | 1| **3**|
|9 | 1| 2|

5 + 5 > 9 finish lay 2, continue lay 3

Begin with no zero, 2 of lay 2,  possible max value is 4, 8, + dp[2][2]

|j\K | 1| 2 | 3 |
|--|--|--| --|
|1 | 1|  |  |
|2 | 1| 1|  |
|3 | 1| 1|  |
|**4** | 1| 2| **1** |
|5 | 1| 1|
|6 | 1| 2|
|7 | 1| 1|
|**8** | 1| 3| **1** |
|9 | 1| 2|

3,  possible max value is 9, + dp[2][3].

|j\K | 1| 2 | 3 |
|--|--|--| --|
|1 | 1|  |  |
|2 | 1| 1|  |
|3 | 1| 1|  |
|4 | 1| 2| 1 |
|5 | 1| 1|
|6 | 1| 2|
|7 | 1| 1|
|8 | 1| 3| 1 |
|**9** | 1| 2| **1** |

4, possible max value is 8, + dp[2][4].

|j\K | 1| 2 | 3 |
|--|--|--| --|
|1 | 1|  |  |
|2 | 1| 1|  |
|3 | 1| 1|  |
|4 | 1| 2| 1 |
|5 | 1| 1|
|6 | 1| 2|
|7 | 1| 1|
|**8** | 1| 3| **2** |
|9 | 1| 2| 1 |

5 + 5 > 9 finish, go continue lay 4

Begin with no zero, 4 of lay 3,  possible max value is 8, + dp[3][4].

|j\K | 1| 2 | 3 |4|
|--|--|--| --| --|
|1 | 1|  |  |
|2 | 1| 1|  |
|3 | 1| 1|  |
|4 | 1| 2| 1 |
|5 | 1| 1|
|6 | 1| 2|
|7 | 1| 1|
|**8** | 1| 3| 2 |**1** |
|9 | 1| 2| 1 | 


After finish build dp, just using simple method to get total possible

```
long ret = 0;
        for (int k = 1; k <= min(18, n); k++)
        {
            for (int i = 1; i <= maxValue; i++)
            {

                ret += dp[k][i] * C[n - 1][k - 1];
                ret %= MOD;
            }
        }

```

A simple full code

```
class Solution {
public:
    int n,m,dp[10005][20];
    int C[10005][20];
    int idealArrays(int n, int m) {
        int mod = 1e9+7;
        for(int i=1; i<=m; i++) {
		    dp[i][1]=(dp[i][1]+1)%mod;
		    for(int j=i+i; j<=m; j+=i) {
			    for(int k=1; k<=18; k++) {
				    dp[j][k]=(dp[j][k]+dp[i][k-1])%mod;
			    }
		    }
	    }
	    for(int i=0; i<=n; i++) {
		    C[i][0]=1;
		    for(int j=1; j<=min(i,18); j++) {
			    C[i][j]=(C[i-1][j]+C[i-1][j-1])%mod;
		    }
	    }
	    long long ans=0;
	    for(int i=1; i<=m; i++) {
		    for(int j=1; j<=18; j++) {
			    ans=(ans+1ll*dp[i][j]*C[n-1][j-1]%mod)%mod;
		    }
	    }
	    return ans;
    }
};
```

**To improve performance, it is recommended to avoid allocating large amounts of memory. using static memory is faster than using a two-dimensional std vector.**
 