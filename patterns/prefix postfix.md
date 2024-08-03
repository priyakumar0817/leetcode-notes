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


## [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/description/)
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
## [724. Find Pivot Index](https://leetcode.com/problems/find-pivot-index/description/)
### Easy
Given an array of integers `nums`, calculate the **pivot index** of this array.  
The **pivot index** is the index where the sum of all the numbers **strictly** to the left of the index is equal to the sum of all the numbers **strictly** to the index's right.  
If the index is on the left edge of the array, then the left sum is `0` because there are no elements to the left. This also applies to the right edge of the array.  
Return _the **leftmost pivot index**_. If no such index exists, return `-1`.  
Example 1:  
**Input:** nums = \[1,7,3,6,5,6\]  
**Output:** 3  
**Explanation:**  
The pivot index is 3.  
Left sum = nums\[0\] + nums\[1\] + nums\[2\] = 1 + 7 + 3 = 11  
Right sum = nums\[4\] + nums\[5\] = 5 + 6 = 11  
Example 2:  
**Input:** nums = \[1,2,3\]  
**Output:** -1  
**Explanation:**  
There is no index that satisfies the conditions in the problem statement.  
Example 3:  
**Input:** nums = \[2,1,-1\]  
**Output:** 0  
**Explanation:**  
The pivot index is 0.  
Left sum = 0 (no elements to the left of index 0)  
Right sum = nums\[1\] + nums\[2\] = 1 + -1 = 0  
**Constraints:**  
-   `1 <= nums.length <= 104`  
-   `-1000 <= nums[i] <= 1000`  
**Note:** This question is the same as 1991: [https://leetcode.com/problems/find-the-middle-index-in-array/](https://leetcode.com/problems/find-the-middle-index-in-array/)
```class Solution {
    public int pivotIndex(int[] nums) {
        int n = nums.length;
        int[] postFix = new int[n + 1];
        int[] preFix = new int[n + 1];
        int sum = 0;
        for (int i = 1; i <= n; i++) {
            sum += nums[i - 1];
            preFix[i - 1] += sum;
        }
        sum = 0;
        for (int i = n - 1; i >= 0; i--) {
            sum += nums[i];
            postFix[i] += sum;
        }
        int preSum = 0;
        for (int i = 1; i <= n; i++) {
            if (preSum == postFix[i]) return i - 1;
            preSum = preFix[i - 1];
        }
        return -1;
    }
}
 ```


## [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/description/)
### Medium
Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.  
The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.  
You must write an algorithm that runs in `O(n)` time and without using the division operation.  
Example 1:  
**Input:** nums = \[1,2,3,4\]  
**Output:** \[24,12,8,6\]  
Example 2:  
**Input:** nums = \[-1,1,0,-3,3\]  
**Output:** \[0,0,9,0,0\]  
**Constraints:**  
-   `2 <= nums.length <= 105`  
-   `-30 <= nums[i] <= 30`  
-   The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.  
**Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not**
```class Solution {
    public int[] productExceptSelf(int[] nums) {
        // note: can do this using a prefix and postfix solution but i didnt in this one
        int prod = 1, countZero = 0, n = nums.length;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                countZero++;
            } else
                prod *= nums[i];
        }
        for (int i = 0; i < n; i++) {
            if (countZero > 1) {
                nums[i] = 0;
            } else if (countZero == 1) {
                if (nums[i] == 0)
                    nums[i] = prod;
                else
                    nums[i] = 0;
            } else {
                nums[i] = prod / nums[i];
            }
        }
        return nums;
    }
}
 ```