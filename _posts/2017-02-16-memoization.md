---
layout: post
title: Memoization in Javascript
thumbnail: /assets/images/javascript.png
tags:
    - javascript
    - memoization
---


**Memoization** is a programming technique which attempts to increase a function’s performance by caching its previously computed results.

```javascript
let count = 0
const fibonacci = (n) => {
    count++
    return n < 2 ? n : fibonacci(n - 1) + fibonacci(n - 2)
}

console.time('#Case1')
console.log(fibonacci(20))
console.log(`call ${count} times`)
console.timeEnd('#Case1')
```
