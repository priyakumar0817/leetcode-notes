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