---
layout: post
title: Memoization in Javascript
thumbnail: /assets/images/memoization.jpg
tags:
    - javascript
    - memoization
    - optimization
---


**Memoization** is a optimization technique which attempts to increase a function’s performance by caching its previously computed results. It is especially useful in recursive functions. Let take a [Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number) for example (A Fibonacci number is the sum of the two previous Fibonacci numbers. The first two are 0 and 1).

Here is the traditional Fibonacci function:

```javascript
// normal way - recursive
let count = 0
const fibonacci = (n) => {
    count++
    return n < 2 ? n : fibonacci(n - 1) + fibonacci(n - 2)
}
console.log(fibonacci(10), count)  // 55 - 177
```

We are using recursive method to calculate the sequence. This works, but the `fibonacci` function is called 177 times. That's maybe a small number, but what if we have to compute `fibonacci(50)` ?! That would take an insane number of times to calculate and give output result. You can try!


### Memoization comes to rescue

We can use Memoization by storing returned values of a function in previous calls, and use them in further calls to the function instead of calling the function itself over and over again and re-calculate values from the beginning. That sounds great!

Here is the re-written code in memoization way:


```javascript
// Memoization way
let count = 0
const fibonacci = (() => {
    let memo = {}
    return function fib(n) {
        count++
        let result
        if(n in memo) {
            result = memo[n]
        } else {
            if(n < 2) {
                result = n
            } else {
                result = fib(n - 1) + fib(n - 2)
            }
        }
        memo[n] = result
        return result
    }
})() // self-execution function
console.log(fibonacci(10), count)  // 55 - 19
```

Now, the function is called 19 times - a huge optimization. You can test with `fibonacci(50)`
, it's much faster than the previous version.

## How did we do it? 

We create `memo` as an empty object to store memoized results. Everytimes the `fib` function is called, it will look for the values in `memo` to find if `memo` already has that value or not. 

+ If Yes -> Return `memo[n]`.
+ If No -> Calculate the value and store it in `memo`.

Finally, we return the result.

## Uh... It's great but it create longer codes...

We can rewrite by using [Conditional (ternary) Operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) to have a cleaner version:

```javascript
let count = 0
const fibonacci = (() => {
    let memo = {}
    return function fib(n) {
        count++
        return n in memo
            ? memo[n] 
            : ( memo[n] = (n < 2 ? n : fib(n - 1) + fib(n - 2)) )
    }
})()
console.log(fibonacci(10), count)  // 55 - 19
```

_The same result -> Good!_

## Generalize memoization function

We can also genalize a memoization function, so that it can be used with other functions, not just `fibonacci` only.

```javascript
const memoizer = (func) => {
    let memo = {}
    return function() {
        var args = Array.prototype.slice.call(arguments)
        return (args in memo) ? memo[args] : ( memo[args] = func.apply(this, args) )
    }   
}

const fibonacci = memoizer(function(n) {
    return n < 2 ? n : fibonacci3(n - 1) + fibonacci3(n - 2)
})

const factorial = memoizer(function(n) {
    return n < 2 ? 1 : n * factorial(n - 1)
})

console.log(fibonacci(10))  // Still 55 - 19 times if you add `count`
console.log(factorial(10))  // 3628800 - factorial function and use memoization too!!!
```


