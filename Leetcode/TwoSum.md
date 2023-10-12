# Two Sum

[Two Sum, Leetcode](https://leetcode.com/problems/two-sum/)

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

**Example 1:**

**Input:** nums = [2,7,11,15], target = 9  
**Output:** [0,1]  
**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2:**

**Input:** nums = [3,2,4], target = 6  
**Output:** [1,2]

**Example 3:**

**Input:** nums = [3,3], target = 6  
**Output:** [0,1]

## JavaScript example

### First solution (two pointers)

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */

 /*
Time complexity: O(n log n)
[Due to the sorting step, where n is the length of the input array]
Space complexity: O(n)
[Due to the creation of the map array, where n is the length of the input array]
 */

var twoSum = function(nums, target) {
    let left = 0;
    let right = nums.length - 1;
    const map = nums.map((value, index) => ({value, index}))
    map.sort((a,b) => a.value - b.value)

    while (left <= right) {
        let sum = map[left].value + map[right].value;

        if (sum == target) return [map[left].index, map[right].index]

        if (sum < target) left++; else right--;
    }

    return [];
};
```

### Second solution
```javascript
var twoSum = function(nums, target) {
    const numToIndex = new Map(); // Create a Map to store numbers and their indices

    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];

        // Check if the complement exists in the Map
        if (numToIndex.has(complement)) {
            return [numToIndex.get(complement), i];
        }

        // Store the current number and its index in the Map
        numToIndex.set(nums[i], i);
    }

    throw new Error("No solution found");
};
```
