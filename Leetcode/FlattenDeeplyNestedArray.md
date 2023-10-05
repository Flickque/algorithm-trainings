# Flatten Deeply Nested Array

[#2625. Flatten Deeply Nested Array, Leetcode](https://leetcode.com/problems/flatten-deeply-nested-array/description/?envType=study-plan-v2&envId=30-days-of-javascript)

Given a multi-dimensional array arr and a depth n, return a flattened version of that array.

A multi-dimensional array is a recursive data structure that contains integers or other multi-dimensional arrays.

A flattened array is a version of that array with some or all of the sub-arrays removed and replaced with the actual elements in that sub-array. This flattening operation should only be done if the current depth of nesting is less than n. The depth of the elements in the first array are considered to be 0.

## JavaScript Example

```javascript

/**
 * @param {Array} arr
 * @param {number} depth
 * @return {Array}
 */
 
// First solution 
// Concat is working bad with big data, spread operator is better

var flat = function(arr, n) {
  let result = [];

  arr.forEach(item => {
    if (Array.isArray(item) && n > 0) {
      result = result.concat(flatten(item));  
    } else {
      result.push(item);
    }
  });

  return result;
}



// Second optimised solution 
var flat = function (arr, n) {
  let result = [];

  arr.forEach(item => {
    if (Array.isArray(item) && n > 0) {
      result.push(...flat(item, n - 1))
    } else {
      result.push(item);
    }
  });

  return result;
}

// Solution with stack
var flat = function(arr, depth) {
  const stack = [...arr.map(item => [item, depth])];
  const result = [];

  while (stack.length > 0) {
    const [item, depth] = stack.pop();

    if (Array.isArray(item) && depth > 0) {
      stack.push(...item.map(subItem => [subItem, depth - 1]));
    } else {
      result.push(item);
    }
  }

  return result.reverse();
};

// With inside function 
var flat = function(arr, n) {
    const res = [];

    function helper(arr,depth) {
        for (const val of arr) {
            if (Array.isArray(val) && depth < n) {
                helper(val, depth + 1);
            } else {
                res.push(val);
            }
        }
        return res;
    }
    return helper(arr, 0);
};