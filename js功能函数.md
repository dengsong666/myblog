## 函数柯里化

柯里化作为一种高阶函数，可以将 `n`元函数转换成 `m`元函数 `（m<=n）`分步执行。

其本质是：将参数利用闭包存起来，数量凑齐了就执行，没凑齐就继续下一轮。

```javascript
/**
 * len：默认值=函数参数个数，对于需要柯里化的函数使用剩余参数写法，需要指定参数个数
 */
function curry(fn: Function, len = fn.length) {
  return function curried(...args: any[]) {
    // 一种写法类似fn.apply(this, args)，这里的this，主要兼容被柯里化的函数this对象指向了其他对象，调用的时候也不能柯里化式分步计算
    // 正常情况下柯里化的函数，this就指向了window，严格模式下undefined，所以apply写法用处不大
    return args.length < len ? (...args2: any[]) => curried(...args, ...args2) : fn(...args);
  };
}

function add(a: number, b: number, c: number) {
  return a + b + c;
}

function add2(...args: number[]) {
  return args.reduce((pre, cur) => pre + cur);
}

console.log(curry(add)(1)(2, 3));
console.log(curry(add2, 4)(1)(2, 3)(4));
```
