# Implement curry() with placeholder support

[#2 Javascript, Bigfrontend](https://bigfrontend.dev/problem/implement-curry-with-placeholder)

This is a follow-up on 1. implement curry()

please implement curry() which also supports placeholder.

Here is an example

```javascript

const  join = (a, b, c) => {
   return `${a}_${b}_${c}`
}

const curriedJoin = curry(join)
const _ = curry.placeholder

curriedJoin(1, 2, 3) // '1_2_3'

curriedJoin(_, 2)(1, 3) // '1_2_3'

curriedJoin(_, _, _)(1)(_, 3)(2) // '1_2_3'

```

## Key Concepts:
### Placeholder Initialization:
- A unique symbol represents the placeholder. This symbol indicates a position where an argument will be provided in a subsequent call.
### Partial Application and Currying:
- Transforms a function of the form f(a, b, c) into one that can be called as f(a)(b)(c).

## Algorithm Steps:
### 1. Initiate Placeholder:
- Check if curry.placeholder already exists. If not, create a unique symbol to represent it.
### 2. Define the Curried Function:
- When curried is invoked:
- Extract the required number of arguments based on the original function's arity.
- If these arguments meet the original function's arity and don't contain placeholders, invoke the original function.
- Otherwise, return a new function waiting for the remaining arguments.
### 3. Merging Arguments:
- In the returned function:
- Combine the originally provided arguments (args) with the new ones (newArgs).
- If an original argument is a placeholder, replace it with a corresponding new argument.
- If the combined arguments are now sufficient and lack placeholders, call the curried function recursively.

## Solution

```javascript

// This is a JavaScript coding problem from BFE.dev 

/**
 * @param { (...args: any[]) => any } fn
 * @returns { (...args: any[]) => any }
 */
function curry(fn) {
  curry.placeholder = curry.placeholder || Symbol();

  function curried(...args) {
      const firstArgs = args.slice(0, fn.length)
      
      if (firstArgs.filter(arg => arg !== curry.placeholder).length >= fn.length) {
          return fn(...args.slice(0, fn.length));
      } else {
          return (...newArgs) => {
              let mergedArgs = [];
              let j = 0;
              for (let i = 0; i < args.length; i++) {
                  if (args[i] === curry.placeholder && j < newArgs.length) {
                      mergedArgs.push(newArgs[j]);
                      j++;
                  } else {
                      mergedArgs.push(args[i]);
                  }
              }

              while (j < newArgs.length) {
                  mergedArgs.push(newArgs[j]);
                  j++;
              }

              return curried(...mergedArgs);
          };
      }
  }

  return curried;
}


curry.placeholder = Symbol()

```