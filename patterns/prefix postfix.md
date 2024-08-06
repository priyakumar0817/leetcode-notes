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

## [304. Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/description/)
### Medium
 Given a 2D matrix `matrix`, handle multiple queries of the following type:  
-   Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.  
Implement the `NumMatrix` class:  
-   `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.  
-   `int sumRegion(int row1, int col1, int row2, int col2)` Returns the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.  
You must design an algorithm where `sumRegion` works on `O(1)` time complexity.  
**Input**  
\["NumMatrix", "sumRegion", "sumRegion", "sumRegion"\]  
\[\[\[\[3, 0, 1, 4, 2\], \[5, 6, 3, 2, 1\], \[1, 2, 0, 1, 5\], \[4, 1, 0, 1, 7\], \[1, 0, 3, 0, 5\]\]\], \[2, 1, 4, 3\], \[1, 1, 2, 2\], \[1, 2, 2, 4\]\]  
**Output**  
\[null, 8, 11, 12\]  
**Explanation**  
NumMatrix numMatrix = new NumMatrix(\[\[3, 0, 1, 4, 2\], \[5, 6, 3, 2, 1\], \[1, 2, 0, 1, 5\], \[4, 1, 0, 1, 7\], \[1, 0, 3, 0, 5\]\]);  
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e sum of the red rectangle)  
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (i.e sum of the green rectangle)  
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (i.e sum of the blue rectangle)  
**Constraints:**  
-   `m == matrix.length`  
-   `n == matrix[i].length`  
-   `1 <= m, n <= 200`  
-   `-104 <= matrix[i][j] <= 104`  
-   `0 <= row1 <= row2 < m`  
-   `0 <= col1 <= col2 < n`  
-   At most `104` calls will be made to `sumRegion`.

```class NumMatrix {
    // O(m*n + queries)
    // O(m*n) memory
    int[][] prefixsum;
    int row, col;
    public NumMatrix(int[][] matrix) {
        row = matrix.length;
        col = matrix[0].length;
        prefixsum = new int[row + 1][col + 1];
        for (int i = 1; i <= row; i++) {
            for (int j = 1; j <= col; j++) {
                prefixsum[i][j] = prefixsum[i - 1][j] + prefixsum[i][j-1] - prefixsum[i-1][j-1] + matrix[i - 1][j -1];
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return prefixsum[row2 + 1][col2 + 1] - prefixsum[row1][col2 + 1] - prefixsum[row2 + 1][col1] + prefixsum[row1][col1];
    }
}
```

## [3070. Count Submatrices with Top-Left Element and Sum Less Than k](https://leetcode.com/problems/count-submatrices-with-top-left-element-and-sum-less-than-k/description/)
### Medium
You are given a **0-indexed** integer matrix `grid` and an integer `k`.  
Return _the **number** of_  
_submatrices_  
 _that contain the top-left element of the_ `grid`, _and have a sum less than or equal to_ `k`.  
Example 1:  
![](https://assets.leetcode.com/uploads/2024/01/01/example1.png)  
**Input:** grid = \[\[7,6,3\],\[6,6,1\]\], k = 18  
**Output:** 4  
**Explanation:** There are only 4 submatrices, shown in the image above, that contain the top-left element of grid, and have a sum less than or equal to 18.  
Example 2:  
![](https://assets.leetcode.com/uploads/2024/01/01/example21.png)  
**Input:** grid = \[\[7,2,9\],\[1,5,0\],\[2,6,6\]\], k = 20  
**Output:** 6  
**Explanation:** There are only 6 submatrices, shown in the image above, that contain the top-left element of grid, and have a sum less than or equal to 20.  
**Constraints:**  
-   `m == grid.length`  
-   `n == grid[i].length`  
-   `1 <= n, m <= 1000`  
-   `0 <= grid[i][j] <= 1000`  
-   `1 <= k <= 109`

```
2 ways:  

class Solution {
    public int countSubmatrices(int[][] grid, int k) {
        int r = grid.length, c = grid[0].length;
        int[][] p = new int[r + 1][c + 1];
        int count = 0;
        for (int i = 1; i <= r; i++) {
            for (int j = 1; j <= c; j++) {
                p[i][j] = p[i - 1][j] + p[i][j - 1] - p[i-1][j-1] + grid[i-1][j-1];
                if (p[i][j] <= k) count++;
            }
        }
        return count;
    }
}

class Solution {
    public int countSubmatrices(int[][] grid, int k) {
        int r = grid.length, c = grid[0].length;
        int[] p = new int[c + 1];
        int count = 0, sum = 0;
        for (int i = 0; i < r; i++) {
            sum = 0;
            for (int j = 0; j < c; j++) {
                sum += grid[i][j];
                p[j] += sum;
                if (p[j] <= k)
                    count++;
            }
        }
        return count;
    }
}
```

## [3096. Minimum Levels to Gain More Points](https://leetcode.com/problems/minimum-levels-to-gain-more-points/description/)
### Medium
You are given a binary array `possible` of length `n`.  
Alice and Bob are playing a game that consists of `n` levels. Some of the levels in the game are **impossible** to clear while others can **always** be cleared. In particular, if `possible[i] == 0`, then the `ith` level is **impossible** to clear for **both** the players. A player gains `1` point on clearing a level and loses `1` point if the player fails to clear it.  
At the start of the game, Alice will play some levels in the **given order** starting from the `0th` level, after which Bob will play for the rest of the levels.  
Alice wants to know the **minimum** number of levels she should play to gain more points than Bob, if both players play optimally to **maximize** their points.  
Return _the **minimum** number of levels Alice should play to gain more points_. _If this is **not** possible, return_ `-1`.  
**Note** that each player must play at least `1` level.  
Example 1:  
**Input:** possible = \[1,0,1,0\]  
**Output:** 1  
**Explanation:**  
Let's look at all the levels that Alice can play up to:  
-   If Alice plays only level 0 and Bob plays the rest of the levels, Alice has 1 point, while Bob has -1 + 1 - 1 = -1 point.  
-   If Alice plays till level 1 and Bob plays the rest of the levels, Alice has 1 - 1 = 0 points, while Bob has 1 - 1 = 0 points.  
-   If Alice plays till level 2 and Bob plays the rest of the levels, Alice has 1 - 1 + 1 = 1 point, while Bob has -1 point.  
Alice must play a minimum of 1 level to gain more points.  
Example 2:  
**Input:** possible = \[1,1,1,1,1\]  
**Output:** 3  
**Explanation:**  
Let's look at all the levels that Alice can play up to:  
-   If Alice plays only level 0 and Bob plays the rest of the levels, Alice has 1 point, while Bob has 4 points.  
-   If Alice plays till level 1 and Bob plays the rest of the levels, Alice has 2 points, while Bob has 3 points.  
-   If Alice plays till level 2 and Bob plays the rest of the levels, Alice has 3 points, while Bob has 2 points.  
-   If Alice plays till level 3 and Bob plays the rest of the levels, Alice has 4 points, while Bob has 1 point.  
Alice must play a minimum of 3 levels to gain more points.  
Example 3:  
**Input:** possible = \[0,0\]  
**Output:** \-1  
**Explanation:**  
The only possible way is for both players to play 1 level each. Alice plays level 0 and loses 1 point. Bob plays level 1 and loses 1 point. As both players have equal points, Alice can't gain more points than Bob.  
**Constraints:**  
-   `2 <= n == possible.length <= 105`  
-   `possible[i]` is either `0` or `1`.
```
class Solution {
    public int minimumLevels(int[] possible) {
        int n = possible.length;
        // total sum
        int sum = 0, min = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) {
            sum += possible[i] == 0 ? -1 : 1;
        }
        // Alice's sum is currsum
        int currsum = 0;
        for (int i = 0; i < n - 1; i++) {
            currsum += possible[i] == 0 ? -1 : 1;
            if (currsum > (sum - currsum))
                return i + 1;
        }
        return -1;

    }
}

```

## [3212. Count Submatrices With Equal Frequency of X and Y](https://leetcode.com/problems/count-submatrices-with-equal-frequency-of-x-and-y/description/)