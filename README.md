# leetcode-notes
Leetcode Study and Practice

[198. House Robber](https://leetcode.com/problems/house-robber/)

Medium  
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.  
Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight **without alerting the police**.  
**Example 1:**  
**Input:** nums = \[1,2,3,1\]  
**Output:** 4  
**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).  
Total amount you can rob = 1 + 3 = 4.  
**Example 2:**  
**Input:** nums = \[2,7,9,3,1\]  
**Output:** 12  
**Explanation:** Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).  
Total amount you can rob = 2 + 9 + 1 = 12.  
**Constraints:**  
-   1 <= nums.length <= 100
-   0 <= nums[i] <= 400

``` 
// top down (TLE)
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        else if (n == 2) return Math.max(nums[0],nums[1]);
        int[] memo = new int[n];
        return rec(n-1,memo, nums);
    }
    private int rec(int n, int[] memo, int[] nums) {
        if (n < 0) return 0;
        if (memo[n] != 0) return memo[n];
        memo[n] = Math.max(rec(n - 1, memo, nums), nums[n] + rec(n - 2, memo, nums));
        return memo[n];
    }
}

// bottom up

class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0],nums[1]);
        for (int i = 2; i < n; i++) {
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);
        }
        return dp[n - 1];
    }
}


```

[746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/description/)

Easy  
You are given an integer array cost where cost\[i\] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.  
You can either start from the step with index 0, or the step with index 1.  
Return the minimum cost to reach the top of the floor.  
**Example 1:**  
**Input:** cost = \[10,15,20\]  
**Output:** 15  
**Explanation:** You will start at index 1.  
\- Pay 15 and climb two steps to reach the top.  
The total cost is 15.  
**Example 2:**  
**Input:** cost = \[1,100,1,1,1,100,1,1,100,1\]  
**Output:** 6  
**Explanation:** You will start at index 0.  
\- Pay 1 and climb two steps to reach index 2.  
\- Pay 1 and climb two steps to reach index 4.  
\- Pay 1 and climb two steps to reach index 6.  
\- Pay 1 and climb one step to reach index 7.  
\- Pay 1 and climb two steps to reach index 9.  
\- Pay 1 and climb one step to reach the top.  
The total cost is 6.  
**Constraints:**  
-   2 <= cost.length <= 1000
-   0 <= cost[i] <= 999
  

```
 // BOTTOM UP: 
 class Solution {
	// O(n) O(n) soln
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length, count = 0;
        int[] dp = new int[n + 1];
        for (int i = 2; i <= n; i++) {
            dp[i] = Math.min(dp[i-2] + cost[i-2], dp[i-1] + cost[i-1]);
        }
        return dp[n];
    }
}
// TOP DOWN:
 class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        int[] dp = new int[n + 1];
        return rec(n, dp, cost);
    }
    private int rec(int n, int[] memo, int[] cost) {
        // base cases
        if (n == 0 || n == 1) return 0;
        if (memo[n] != 0) return memo[n];
        memo[n] = Math.min(cost[n - 1] + rec(n - 1, memo, cost), cost[n - 2] + rec(n - 2, memo, cost));
        return memo[n];
    }
}
// BOTTOM UP O(1); 
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        // O(n) O(1) solution
        int n = cost.length, firstPrev = 0, secPrev = 0, curr = 0;
        for (int i = 2; i <= n; i++) {
            curr = Math.min(firstPrev + cost[i-1], secPrev + cost[i-2]);
            secPrev = firstPrev;
            firstPrev = curr;
        }
        return curr;
    }
}

```
## Delete and Earn  
Medium  
You are given an integer array nums. You want to maximize the number of points you get by performing the following operation any number of times:  
-   Pick any nums\[i\] and delete it to earn nums\[i\] points. Afterwards, you must delete **every** element equal to nums\[i\] - 1 and **every** element equal to nums\[i\] + 1.  
Return the **maximum number of points** you can earn by applying the above operation some number of times.  
**Example 1:**  
**Input:** nums = \[3,4,2\]  
**Output:** 6  
**Explanation:** You can perform the following operations:  
\- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = \[2\].  
\- Delete 2 to earn 2 points. nums = \[\].  
You earn a total of 6 points.  
**Example 2:**  
**Input:** nums = \[2,2,3,3,3,4\]  
**Output:** 9  
**Explanation:** You can perform the following operations:  
\- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = \[3,3\].  
\- Delete a 3 again to earn 3 points. nums = \[3\].  
\- Delete a 3 once more to earn 3 points. nums = \[\].  
You earn a total of 9 points.  
 **Constraints:**  
-   1 <= nums.length <= 2 \* 104  
-   1 <= nums\[i\] <= 104
``` 

BETTER SOLUTION:class Solution {
    public int deleteAndEarn(int[] nums) {
        int n = nums.length;
        int max = 0;
        for (int num : nums) {
            max = Math.max(max, num);
        }
        int[] dp = new int[max + 1];
        for (int num : nums) {
            dp[num] += num;
        }
        for (int i = 2; i <= max; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i] + dp[i - 2]);
        }
        return dp[max];
    }
}

MY SOLUTION: 
class Solution {
    public int deleteAndEarn(int[] nums) {
        int n = nums.length;
        if (n == 1)
            return nums[0];
        int[] freqs = new int[10001];
        int[] dp = new int[n];
        int max = 0;
        int min = 100001, min2 = 100001;
        for (int num : nums) {
            freqs[num]++;
            max = Math.max(max, num);
            if (num < min) {
                min2 = min;
                min = num;
            } else if (num < min2 && num != min) {
                min2 = num;
            }
        }
        dp[0] = min * freqs[min];
        if (min2 == 100001)
            return dp[0];
        if (min == min2 - 1) {
            dp[1] = Math.max(freqs[min2] * min2, freqs[min] * min);
        } else {
            dp[1] = (freqs[min] * min) + (freqs[min2] * min2);
        }
        int j = 2;
        for (int i = min2 + 1; i <= max; i++) {
            if (freqs[i] != 0) {
                int gain = freqs[i] * i;
                if (freqs[i - 1] != 0) {
                    dp[j] = Math.max(dp[j - 1], dp[j - 2] + gain);
                } else {
                    dp[j] = dp[j - 1] + gain;
                }
                j++;
            }
        }
        return dp[j-1];
    }
}
 ```

[1770. Maximum Score from Performing Multiplication Operations](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/description/)  
Hard  
You are given two **0-indexed** integer arrays nums and multipliers of size n and m respectively, where n >= m.  
You begin with a score of 0. You want to perform **exactly** m operations. On the ith operation (**0-indexed**) you will:  
-   Choose one integer x from **either the start or the end** of the array nums.  
-   Add multipliers\[i\] \* x to your score.  
-   Note that multipliers\[0\] corresponds to the first operation, multipliers\[1\] to the second operation, and so on.  
-   Remove x from nums.  
Return the **maximum** score after performing m operations.  
**Example 1:**  
**Input:** nums = \[1,2,3\], multipliers = \[3,2,1\]  
**Output:** 14  
**Explanation:** An optimal solution is as follows:  
\- Choose from the end, \[1,2,**3**\], adding 3 \* 3 = 9 to the score.  
\- Choose from the end, \[1,**2**\], adding 2 \* 2 = 4 to the score.  
\- Choose from the end, \[**1**\], adding 1 \* 1 = 1 to the score.  
The total score is 9 + 4 + 1 = 14.  
**Example 2:**  
**Input:** nums = \[-5,-3,-3,-2,7,1\], multipliers = \[-10,-5,3,4,6\]  
**Output:** 102  
**Explanation:** An optimal solution is as follows:  
\- Choose from the start, \[**\-5**,-3,-3,-2,7,1\], adding -5 \* -10 = 50 to the score.  
\- Choose from the start, \[**\-3**,-3,-2,7,1\], adding -3 \* -5 = 15 to the score.  
\- Choose from the start, \[**\-3**,-2,7,1\], adding -3 \* 3 = -9 to the score.  
\- Choose from the end, \[-2,7,**1**\], adding 1 \* 4 = 4 to the score.  
\- Choose from the end, \[-2,**7**\], adding 7 \* 6 = 42 to the score.  
The total score is 50 + 15 - 9 + 4 + 42 = 102.  
**Constraints:**  
-   n == nums.length  
-   m == multipliers.length  
-   1 <= m <= 300  
-   m <= n <= 105  
-   \-1000 <= nums\[i\], multipliers\[i\] <= 1000
 ```
 TOP DOWN: class Solution {
    public int maximumScore(int[] nums, int[] multipliers) {
        int m = multipliers.length;
        int[][] memo = new int[m][m];
        return rec(0, 0, nums, multipliers, memo);
    }
    private int rec(int i, int l, int[] nums, int[] mul, int[][] memo) {
        if (i == mul.length) return 0;
        if (memo[l][i] != 0) return memo[l][i];
        int right = nums.length - (i - l) - 1;
        memo[l][i] = Math.max(mul[i] * nums[l] + rec(i + 1, l + 1, nums, mul, memo), mul[i] * nums[right] + rec(i + 1, l, nums, mul, memo));
        return memo[l][i];
    }
 }
 BOTTOM UP: class Solution {
    public int maximumScore(int[] nums, int[] multipliers) {
        int m = multipliers.length, n = nums.length;
        int[][] dp = new int[m + 1][m + 1];
        int left = 0, right = 0;
        for (int i = 0; i <= m; i++) {
            for (left = 0; left < i; left++) {
                right = n - (i - left) - 1;
                dp[i][left] = Math.max(multipliers[i] * nums[left] + dp[i + 1][left + 1],
                        multipliers[i] * nums[right] + dp[i + 1][left]);


            }
        }
        return dp[0][0];
    }
}

```