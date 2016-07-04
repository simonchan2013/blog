# 函数
## Function类型
#### 注意点
- 函数实际上是对象，每个函数都是Function类型的实例
- 函数名实际上是一个指向函数的指针，不与某个函数绑定。（因此一个函数可以有多个函数名）

````js
function sum(n1, n2) {
  return n1 + n2;
}

console.log(sum(10, 10)); // => 20

var sum2 = sum;
console.log(sum2(10, 10)); // => 20

sum = null;
console.log(sum2(10, 10)); // => 20
````
- 没有重载
- 函数内部有两个特殊的对象：arguments和this。arguments中有一个callee属性，是一个指针，指向拥有这个arguments对象的函数。（在递归中使用callee可以避免函数依赖函数名）

````js
// 'use strict'

function factorial(num) {
  if (num <= 1) { return 1; }
  else { return num * arguments.callee(num-1); } // arguments.callee 指向 factorial 函数，严格模式下会出错
}

// 严格模式下替代方案
// var factorial = function f(num) {
//   if (num <= 1) { return 1; }
//   else { return num * f(num-1); }
// };

console.log(factorial(-1)); // => 1
console.log(factorial(0));  // => 1
console.log(factorial(1));  // => 1
console.log(factorial(2));  // => 2
console.log(factorial(3));  // => 6
console.log(factorial(4));  // => 24
console.log(factorial(10)); // => 3628800



var anotherFactorial = factorial;

factorial = function() {
  return 'I am an new function now!';
}

console.log(anotherFactorial(-1)); // => 1
console.log(anotherFactorial(0));  // => 1
console.log(anotherFactorial(1));  // => 1
console.log(anotherFactorial(10)); // => 3628800
console.log(factorial(10));        // => I am an new function now!
````
- caller属性：指向调用当前函数的函数的引用，也可通过arguments.callee.caller访问。

````js
function outer() {
  inner();
}

function inner() {
  console.info(inner.caller);             // => [Function: outer]

  console.info(arguments.callee.caller);  // => [Function: outer]
}

outer();
````
- length属性：函数的参数个数
- 两个非继承的方法：apply()和call()。都是用于在指定的作用域中调用函数，两者的区别仅在于接收的参数不同。

````js
function sum(n1, n2) { return n1+n2; }

function applySum1(n1, n2) { return sum.apply(this, arguments); }
function applySum2(n1, n2) { return sum.apply(this, [n1, n2]); }
function callSum(n1, n2) { return sum.call(this, n1, n2); }

console.log(applySum1(10, 10)); // => 20
console.log(applySum2(15, 15)); // => 30
console.log(callSum(25, 25));   // => 50



this.color = 'red';
var o = { color: 'blue' };

function sayColor() { console.log(this.color); }

// sayColor();
sayColor.call(this);  // => red
sayColor.apply(this); // => red
sayColor.call(o);     // => blue
sayColor.apply(o);    // => blue
````

#### 函数声明提升
- 执行代码之前会读取函数声明
- 匿名函数：创建一个函数后赋值给一个变量，该变量不会提升

````js
test1(); // => this is test1
// 函数声明
function test1() {
  console.log('this is test1');
}


test2(); // => TypeError: undefined is not a function
// 匿名函数
var test2 = function() {
  console.log('this is test2');
}
````
