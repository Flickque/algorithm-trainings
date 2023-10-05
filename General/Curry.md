# Curry

Basic Curry

## JavaScript Example

```javascript

// First solution
function curry(func) {

  return function curried(...args) {
    if (args.length >= func.length) {
      return func(...args);
    } else {
      return function(...args2) {
        return curried(...args.concat(args2));
      }
    }
  };

}


const sum = curry((a, b, c) => a + b + c);

console.log(sum(1)(2,3))




