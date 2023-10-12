# Decoded String at Index

[#880. Decoded String at Indexy, Leetcode](https://leetcode.com/problems/decoded-string-at-index/description/)

**Objective:**
Given an encoded string `s`, decode the string. When a digit `d` is encountered, the current content of the decoded string is repeated `d-1` more times. Our goal is to find the character at the `k`th position in the decoded string.

## Complexity Analysis:
- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

## Algorithm:

### 1. Calculate the Length of Decoded String:
- Iterate over the string `s`.
    - If the character is a letter, increment the length by 1.
    - If the character is a digit, multiply the current length by this digit.
- Continue this until the length becomes greater than or equal to `k`.

### 2. Reverse Decoding:
- Starting from the last encountered character, iterate backward.
    - If the character is a letter:
        - Check if `k` equals the current length or if `k` is 0. If true, this is our target letter.
        - Otherwise, decrement the length.
    - If the character is a digit:
        - Divide the current length by this digit.
        - Update `k` as `k % length`.

The algorithm finally returns the identified letter from the decoded string at position `k`.


## JavaScript Example

### First Solution
```javascript

/**
 * @param {string} s
 * @param {number} k
 * @return {string}
 */
const decodeAtIndex = function(s, k) {
  let stringLength = 0;
  let i = 0;

  for (i = 0; i<=s.length; i++){
    if (isNaN(s[i])){
      stringLength++;
    } else {
      stringLength *= Number(s[i]);
    }

    if (stringLength >= k) break;
  }

  while (true){
    if (isNaN(s[i])){
      
      if (k === stringLength || k === 0) return s[i]
      else stringLength--;
    } else {
      stringLength /= s[i];
      k = k % stringLength;
    }

    i--;
  }
};

```

### Second brute-force solution
```javascript
const checkString = (char) => {
  if (/[a-zA-Z]/.test(char)) {
    return true;
  } else if (/[0-9]/.test(char)) {
    return false;
  }
}

var decodeAtIndex = function(s, k) {
  let result = [];

  for (let item of s){
    
    if (result.length === k) return result[k-1];

    if (checkString(item)) result.push(item);
    else if (!!checkString){
      result = result.join('').repeat(item).split('');
    }
  }

  return result[k-1]

};
```