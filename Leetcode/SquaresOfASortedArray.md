# 977. Squares of a Sorted Array

[977. Squares of a Sorted Array, Leetcode](https://leetcode.com/problems/squares-of-a-sorted-array/description/)

Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

**Input:** nums = [-4,-1,0,3,10]
**Output:** [0,1,9,16,100]
**Explanation:** After squaring, the array becomes [16,1,0,9,100]. After sorting, it becomes [0,1,9,16,100].

**Follow up:** Squaring each element and sorting the new array is very trivial, could you find an O(n) solution using a different approach?

#Two pointer

## JavaScript Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */

 // Time: O(n), Space: O(1)
var sortedSquares = function(nums) {
    const result = [];
    let left = 0;
    let right = nums.length - 1;

    if (nums.length === 1) return nums.map((item) => Math.pow(item, 2))
    
    while (left <= right) {
        const leftPow = Math.pow(nums[left], 2)
        const rightPow = Math.pow(nums[right], 2)

        if (rightPow > leftPow){
            result.unshift(rightPow)
            right--;
        } else {
            result.unshift(leftPow)
            left++;
        }
    }

    return result;
};
```