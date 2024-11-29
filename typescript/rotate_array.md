# [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

## Approach 1: Using Extra Array

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    rotate(nums: number[], k: number): void {
        const n = nums.length;
        k = k % n; // Handle cases where k > n
        const rotated: number[] = new Array(n);

        // Copy rotated elements to a new array
        for (let i = 0; i < n; i++) {
            rotated[(i + k) % n] = nums[i];
        }

        // Copy elements back to the original array
        for (let i = 0; i < n; i++) {
            nums[i] = rotated[i];
        }
    }
}
```

## Approach 2: Rotate One-by-One (Brute Force)

### Solution
typescript
```typescript
// Time Complexity: O(n * k)
// Space Complexity: O(1)
class Solution {
    rotate(nums: number[], k: number): void {
        const n = nums.length;
        k = k % n; // Handle cases where k > n

        // Rotate the array k times
        for (let i = 0; i < k; i++) {
            const last = nums[n - 1]; // Save the last element
            // Shift elements to the right
            for (let j = n - 1; j > 0; j--) {
                nums[j] = nums[j - 1];
            }
            nums[0] = last; // Place the saved element at the beginning
        }
    }
}
```

## Approach 3: Reverse Array (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    rotate(nums: number[], k: number): void {
        const n = nums.length;
        k = k % n; // Handle cases where k > n

        // Step 1: Reverse the entire array
        this.reverse(nums, 0, n - 1);

        // Step 2: Reverse the first k elements
        this.reverse(nums, 0, k - 1);

        // Step 3: Reverse the remaining n - k elements
        this.reverse(nums, k, n - 1);
    }

    private reverse(nums: number[], start: number, end: number): void {
        while (start < end) {
            const temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```

## Approach 4: Cyclic Replacements

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    rotate(nums: number[], k: number): void {
        const n = nums.length;
        k = k % n; // Handle cases where k > n
        let count = 0; // Number of elements rotated

        for (let start = 0; count < n; start++) {
            let current = start;
            let prev = nums[start];

            do {
                const next = (current + k) % n;
                const temp = nums[next];
                nums[next] = prev;
                prev = temp;
                current = next;
                count++;
            } while (start !== current); // Cycle ends when we return to the start
        }
    }
}
```
