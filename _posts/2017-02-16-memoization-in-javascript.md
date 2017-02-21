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

<script src="https://gist.github.com/mikunpham/6f4c2ec6bc79f54dc9209db2d8a13d2a.js"></script>

We are using recursive method to calculate the sequence. This works, but the `fibonacci` function is called 177 times. That's maybe a small number, but what if we have to compute `fibonacci(50)` ?! That would take an insane number of times to calculate and give output result. You can try!


### Memoization comes to rescue

We can use Memoization by storing returned values of a function in previous calls, and use them in further calls to the function instead of calling the function itself over and over again and re-calculate values from the beginning. That sounds great!

Here is the re-written code in memoization way:

<script src="https://gist.github.com/mikunpham/8f46c96ba8e56dbe8ad4ec945c2cfe71.js"></script>

Now, the function is called 19 times - a huge optimization. You can test with `fibonacci(50)`
, it's much faster than the previous version.

## How did we do it? 

We create `memo` as an empty object to store memoized results. Everytimes the `fib` function is called, it will look for the values in `memo` to find if `memo` already has that value or not. 

+ If Yes -> Return `memo[n]`.
+ If No -> Calculate the value and store it in `memo`.

Finally, we return the result.

## Uh... It's great but it create longer codes...

We can rewrite by using [Conditional (ternary) Operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) to have a cleaner version:

<script src="https://gist.github.com/mikunpham/d733633be3e9e87fc511dcf3fcf0ef0d.js"></script>

_The same result -> Good!_

## Generalize memoization function

We can also genalize a memoization function, so that it can be used with other functions, not just `fibonacci` only.

<script src="https://gist.github.com/mikunpham/1e9108f12883405f4ff3d6cf6913ddbc.js"></script>

## Limitations

[https://www.sitepoint.com/implementing-memoization-in-javascript/](https://www.sitepoint.com/implementing-memoization-in-javascript/)

+ There are several things which must be kept in mind when implementing memoization. First, by storing old results, memoized functions consume additional memory.
+ The biggest limitation of memoization is that it can only be automated with functions that are referentially transparent. A function is considered referentially transparent if its output depends only on its inputs, and it does not cause any side effects.  A call to a referentially transparent function can be replaced by its return value without changing the semantics of the program.  The Fibonacci function is referentially transparent because it depends solely on the value of “n”. In the following example, the function foo() is not referentially transparent because it uses a global variable, “bar”. Since “bar” can be modified outside of foo(), there is no guarantee that the return value will remain the same for each input value. In this example, the two calls to foo() return the values two and three, even though the same arguments are passed to both calls.

<script src="https://gist.github.com/mikunpham/5858d1ea97cefdd0e5b690a42cf9bdc3.js"></script>


