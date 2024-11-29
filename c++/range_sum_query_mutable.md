# 307. [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity for update: O(1)
// Time Complexity for sumRange: O(n)
// Space Complexity: O(n)
class NumArray {
private:
    vector<int> nums;

public:
    NumArray(vector<int>& nums) {
        this->nums = nums;
    }

    void update(int index, int val) {
        // Directly update the value at the given index
        nums[index] = val;
    }

    int sumRange(int i, int j) {
        int sum = 0;
        // Calculate the sum from index i to j
        for (int k = i; k <= j; k++) {
            sum += nums[k];
        }
        return sum;
    }
};
```

## Approach 2: Segment Tree

### Solution
```cpp
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
class NumArray {
private:
    vector<int> nums;
    vector<int> segmentTree;

    void buildTree(int treeIndex, int left, int right) {
        if (left == right) {
            // Leaf node stores the original array value
            segmentTree[treeIndex] = nums[left];
            return;
        }
        int mid = left + (right - left) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        buildTree(leftChild, left, mid);
        buildTree(rightChild, mid + 1, right);
        // Current node store the sum of both segments
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }

    void updateRecursive(int treeIndex, int left, int right, int index, int val) {
        if (left == right) {
            // Update the value in the segment tree and the original array
            segmentTree[treeIndex] = val;
            nums[index] = val;
            return;
        }
        int mid = left + (right - left) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        if (index <= mid) {
            updateRecursive(leftChild, left, mid, index, val);
        } else {
            updateRecursive(rightChild, mid + 1, right, index, val);
        }
        // Recalculate the current node's sum after the update
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }

    int sumRangeRecursive(int treeIndex, int left, int right, int i, int j) {
        if (left > j || right < i) {
            return 0; // Range completely outside
        }
        if (i <= left && right <= j) {
            return segmentTree[treeIndex]; // Range completely inside
        }
        int mid = left + (right - left) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        // Partial overlap
        return sumRangeRecursive(leftChild, left, mid, i, j) +
               sumRangeRecursive(rightChild, mid + 1, right, i, j);
    }

public:
    NumArray(vector<int>& nums) {
        this->nums = nums;
        int n = nums.size();
        segmentTree.resize(4 * n);
        buildTree(0, 0, n - 1);
    }

    void update(int index, int val) {
        updateRecursive(0, 0, nums.size() - 1, index, val);
    }

    int sumRange(int i, int j) {
        return sumRangeRecursive(0, 0, nums.size() - 1, i, j);
    }
};
```

## Approach 3: Binary Indexed Tree (Fenwick Tree)

### Solution
```cpp
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
class NumArray {
private:
    vector<int> nums;
    vector<int> bit;
    int n;

    void init(int index, int val) {
        index++;
        while (index <= n) {
            // Update the binary indexed tree with the value
            bit[index] += val;
            index += index & -index;
        }
    }

    int getSum(int index) {
        int sum = 0;
        index++;
        while (index > 0) {
            sum += bit[index]; // Accumulate the sum from the BIT
            index -= index & -index;
        }
        return sum;
    }

public:
    NumArray(vector<int>& nums) {
        this->nums = nums;
        n = nums.size();
        bit.resize(n + 1, 0);
        for (int i = 0; i < n; i++) {
            init(i, nums[i]);
        }
    }

    void update(int index, int val) {
        int delta = val - nums[index];
        nums[index] = val; // Update the original array
        init(index, delta); // Update the BIT with the difference
    }

    int sumRange(int i, int j) {
        return getSum(j) - getSum(i - 1);
    }
};
```
