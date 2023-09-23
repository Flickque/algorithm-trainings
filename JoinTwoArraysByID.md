# Join Two Arrays by ID

[#2722. Join Two Arrays by ID, Leetcode](https://leetcode.com/problems/join-two-arrays-by-id/?envType=study-plan-v2&envId=30-days-of-javascript)

Given two arrays arr1 and arr2, return a new array joinedArray. All the objects in each of the two inputs arrays will contain an id field that has an integer value. joinedArray is an array formed by merging arr1 and arr2 based on their id key. The length of joinedArray should be the length of unique values of id. The returned array should be sorted in ascending order based on the id key.

If a given id exists in one array but not the other, the single object with that id should be included in the result array without modification.

If two objects share an id, their properties should be merged into a single object:

If a key only exists in one object, that single key-value pair should be included in the object.
If a key is included in both objects, the value in the object from arr2 should override the value from arr1.

## JavaScript Example

```javascript
/**
 * @param {Array} arr1
 * @param {Array} arr2
 * @return {Array}
 */
 
// First solution 
var join = function(arr1, arr2) {

  const result = new Map();


  [...arr1, ...arr2].forEach((item) => {
    const id = item.id;

    if (result.has(id)) {
      const existedItem = result.get(id);
      result.set(id, {...existedItem, ...item})
    } else result.set(id,  {...item})

  })

  return Array.from(result.values()).sort((a,b) => a.id - b.id);
};

// Second optimised solution 
var join = function(arr1, arr2) {
    const result = {};

    // 1. initialization
    arr1.forEach(item => {
        result[item.id] = item;
    });
    // 2. joining
    arr2.forEach(item => {
        if (result[item.id]) {
            Object.keys(item).forEach(key => {
                result[item.id][key] = item[key];    
            });
        } else {
            result[item.id] = item;
        }
    });

    return Object.values(result);
};