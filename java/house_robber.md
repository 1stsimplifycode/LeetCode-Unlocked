# 198. [House Robber](https://leetcode.com/problems/house-robber/)

## Approach 1: Recursive Solution

### Solution
```java
// Time Complexity: O(2^n)
// Space Complexity: O(n)
public class Solution {
    public int rob(int[] nums) {
        return robFrom(0, nums);
    }

    private int robFrom(int index, int[] nums) {
        // Base case: If index is out of bounds, return 0
        if (index >= nums.length) {
            return 0;
        }
        
        // Either rob this house and skip one, or skip it
        int robCurrent = nums[index] + robFrom(index + 2, nums);
        int skipCurrent = robFrom(index + 1, nums);

        // Return the maximum of both choices
        return Math.max(robCurrent, skipCurrent);
    }
}
```

## Approach 2: Recursion with Memoization

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int rob(int[] nums) {
        return robFrom(0, nums, new HashMap<>());
    }

    private int robFrom(int index, int[] nums, HashMap<Integer, Integer> memo) {
        // Base case: If index is out of bounds, return 0
        if (index >= nums.length) {
            return 0;
        }

        // Check memoization table
        if (memo.containsKey(index)) {
            return memo.get(index);
        }

        // Either rob this house and skip one, or skip it
        int robCurrent = nums[index] + robFrom(index + 2, nums, memo);
        int skipCurrent = robFrom(index + 1, nums, memo);

        // Memoize the result
        int result = Math.max(robCurrent, skipCurrent);
        memo.put(index, result);

        return result;
    }
}
```

## Approach 3: Dynamic Programming (Bottom Up)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) return 0;

        int[] dp = new int[nums.length + 1];
        dp[0] = 0;
        dp[1] = nums[0];

        // Build the dp array
        for (int i = 2; i <= nums.length; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i - 1]);
        }

        return dp[nums.length];
    }
}
```

## Approach 4: Optimized Dynamic Programming (Constant Space)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) return 0;

        int prev1 = 0; // max money can rob up to the previous house
        int prev2 = 0; // max money can rob up to two houses before

        // Iterate through each house
        for (int num : nums) {
            int temp = prev1;
            prev1 = Math.max(prev2 + num, prev1);
            prev2 = temp;
        }

        return prev1;
    }
}
```
