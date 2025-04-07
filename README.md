# Algo Template
## Overview
本质：数据结构和算法的排列组合

### 数据结构
数据结构的存储方式只有两种：数组和链表
- [x] 数组：顺序存储
- [x] 链表：链式存储

数据结构的访问方式：遍历 + 访问
- [x] 具体：增删改查

### 算法：核心只有一个：穷举
- [x] 聪明的穷举
- [x] 笨一点的穷举

## Details
### 数据结构 (2x5)
- [x] Array / Vector / String: 数组
- [x] LinkedList

- [x] Map
- [x] Set

- [x] Stack
- [x] Queue

- [x] Priority Queue
- [x] Deque

- [x] Tree
- [x] Graph

### 算法 (3x4)
- [x] BFS
- [x] DFS
- [x] Backtracking: 回溯

- [x] Sorting
- [x] Binary Search
- [x] Two Pointers

- [x] Divide and conquer
- [x] Greedy Algorithm 
- [x] Dynamic Programming

- [x] Prefix Sum
- [x] Union Find
- [x] Monotonic Stack

---

## Two Pointers
### Common patterns:
1. Opposite ends (left/right): Used for **searching** in sorted arrays, partition operations
2. Same direction (slow/fast): Used for **in-place** operations, linked list problems
3. Multiple arrays: Used for **merging** or **comparing** multiple arrays/sequences

```cpp
//---------- Generic Two Pointers Template ----------//
class TwoPointersTemplate {
public:
    void solve(vector<int>& nums) {
        // 1. Initialize pointers based on the problem
        int left = 0;
        int right = nums.size() - 1;
        
        // 2. Define termination condition
        while (left < right) {
            // 3. Process current elements
            // ...
            
            // 4. Update pointers based on condition
            if (/* some condition */) {
                left++;
            } else {
                right--;
            }
        }
        
        // 5. Return the result
        // return ...
    }
    
    // Slow-fast pointers template
    void solveWithSlowFast(vector<int>& nums) {
        int slow = 0;
        
        for (int fast = 0; fast < nums.size(); fast++) {
            // Process current elements
            // ...
            
            // Update slow pointer conditionally
            if (/* some condition */) {
                // Often involves copying or modifying the array
                nums[slow] = nums[fast];
                slow++;
            }
        }
        
        // Further processing if needed
        // ...
    }
};

class OppositeDirectionPointers {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        
        while (left < right) {
            int sum = nums[left] + nums[right];
            
            if (sum == target) {
                return { left + 1, right + 1 };
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        
        return { -1, -1  };
    }

    // Example: Reverse String
    void reverseString(vector<char>& s) {
        int left = 0;
        int right = s.size() - 1;
        
        while (left < right) {
            swap(s[left], s[right]);
            left++;
            right--;
        }
    }

    // Example: Container With Most Water
    int maxArea(vector<int>& height) {
        int left = 0;
        int right = height.size() - 1;
        int maxWater = 0;
        
        while (left < right) {
            int width = right - left;
            int h = min(height[left], height[right]);
            maxWater = max(maxWater, width * h);
            
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        
        return maxWater;
    }
};


//---------- Pattern 2: Same-Direction Pointers ----------//
class SameDirectionPointers {
public:
    // Example: Remove duplicates from sorted array
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        int slow = 0;
        
        for (int fast = 1; fast < nums.size(); fast++) {
            if (nums[fast] != nums[slow]) {
                slow++;
                nums[slow] = nums[fast];
            }
        }
        
        return slow + 1; // Length of new array
    }
    
    // Example: Move Zeroes
    void moveZeroes(vector<int>& nums) {
        int nonZeroPos = 0;
        
        // First pass: move non-zero elements to the front
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != 0) {
                nums[nonZeroPos++] = nums[i];
            }
        }
        
        // Second pass: fill the rest with zeroes
        for (int i = nonZeroPos; i < nums.size(); i++) {
            nums[i] = 0;
        }
    }
};

//---------- Pattern 3: Multiple Arrays Pointers ----------//
class MultipleArrayPointers {
public:
    // Example: Merge sorted arrays (your example)
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1;      // Pointer for nums1
        int j = n - 1;      // Pointer for nums2
        int k = m + n - 1;  // Pointer for result (placed in nums1)
        
        while (i >= 0 || j >= 0) {
            if (i >= 0 && j >= 0) {
                if (nums1[i] >= nums2[j]) {
                    nums1[k--] = nums1[i--];
                } else {
                    nums1[k--] = nums2[j--];
                }
            } else if (i >= 0) {
                nums1[k--] = nums1[i--];
            } else {
                nums1[k--] = nums2[j--];
            }
        }
    }
    
    // Example 1: Merge two sorted arrays with extra space
    vector<int> mergeSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector<int> result(nums1.size() + nums2.size());
        int i = 0;      // Pointer for nums1
        int j = 0;      // Pointer for nums2
        int k = 0;      // Pointer for result
        
        // Merge elements from both arrays in sorted order
        while (i < nums1.size() || j < nums2.size()) {
            if (i >= nums1.size()) {
                result[k++] = nums2[j++];
            }
            else if (j >= nums2.size()) {
                result[k++] = nums1[i++];
            }
            else if (nums1[i] <= nums2[j]) {
                result[k++] = nums1[i++];
            }
            else {
                result[k++] = nums2[j++];
            }
        }
        
        return result;
    }
}

```
