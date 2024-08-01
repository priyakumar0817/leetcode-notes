## [1480. Running Sum of 1d Array](https://leetcode.com/problems/running-sum-of-1d-array/description/)
### Easy
Given an array `nums`. We define a running sum of an array as `runningSum[i] = sum(nums[0]…nums[i])`.  
Return the running sum of `nums`.  
Example 1:  
**Input:** nums = \[1,2,3,4\]  
**Output:** \[1,3,6,10\]  
**Explanation:** Running sum is obtained as follows: \[1, 1+2, 1+2+3, 1+2+3+4\].  
Example 2:  
**Input:** nums = \[1,1,1,1,1\]  
**Output:** \[1,2,3,4,5\]  
**Explanation:** Running sum is obtained as follows: \[1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1\].  
Example 3:  
**Input:** nums = \[3,1,2,10,1\]  
**Output:** \[3,4,6,16,17\]  
**Constraints:**  
-   `1 <= nums.length <= 1000`  
-   `-10^6 <= nums[i] <= 10^6`

```class Solution {
    public int[] runningSum(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            nums[i]+=nums[i-1];
        }
        return nums;
    }
}
 ```


[303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/description/)
### Easy
Given an integer array `nums`, handle multiple queries of the following type:  
1.  Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.  
Implement the `NumArray` class:  
-   `NumArray(int[] nums)` Initializes the object with the integer array `nums`.  
-   `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).  
Example 1:  
**Input**  
\["NumArray", "sumRange", "sumRange", "sumRange"\]  
\[\[\[-2, 0, 3, -5, 2, -1\]\], \[0, 2\], \[2, 5\], \[0, 5\]\]  
**Output**  
\[null, 1, -1, -3\]  
**Explanation**  
NumArray numArray = new NumArray(\[-2, 0, 3, -5, 2, -1\]);  
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1  
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1  
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3  
**Constraints:**  
-   `1 <= nums.length <= 104`  
-   `-105 <= nums[i] <= 105`  
-   `0 <= left <= right < nums.length`  
-   At most `10^4` calls will be made to `sumRange`.
```class NumArray {
    int[] prefixSum;
    public NumArray(int[] nums) {
        int n = nums.length;
        prefixSum = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            prefixSum[i] = nums[i - 1] + prefixSum[i - 1];
        }
    }
    
    public int sumRange(int left, int right) {
        return prefixSum[right + 1] - prefixSum[left];
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(left,right);
 */
 ```