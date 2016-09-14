
### 要点
- `this`是动态的，在函数运行时确定，而不是函数声明定义时（箭头函数除外）
- 所有函数都可以调用`this`，这无关于该函数是否属于某个对象

### `this`的使用情况
1. 当函数是某一对象的方法
  - `this`指向该对象
  - 当一个对象的方法被取出来赋值给一个变量时，该变量所表示的函数中，`this`指向`window`或`undefined`（严格模式）
  - 当在对象中的方法中定义函数时，此函数的`this`指向`window`或`undefined`（严格模式），可以通过`call`、`apply`或`bind`重新绑定

  ```js
  var name = 'John';

  var obj = {
    name: 'Simon',
    sayHi: function() {
      console.log('Hi, ' + this.name);
    },
    sayYeah: function() {
      function truelySayYeah() {
        console.log('Yeah, ' + this.name);  // this指向window
      }
      return truelySayYeah();
    },
    sayYo: function() {
      function truelySayYo() {
        console.log('Yo, ' + this.name);  // 绑定后，this指向obj
      }

      // 以下方法均可重新绑定
      // return truelySayYo.bind(this)(); // => bind会返还新函数
      // return truelySayYo.call(this);
      return truelySayYo.apply(this);
    }
  }
  function sayBye() {
    console.log('Bye, ' + this.name);
  }
  obj.sayBye = sayBye;
  var sayHi = obj.sayHi;

  sayHi();        // => Hi, John
  sayBye();       // => Bye, John
  obj.sayHi();    // => Hi, Simon
  obj.sayBye();   // => Bye, Simon
  obj.sayYeah();  // => Yeah, John
  obj.sayYo();    // => Yo, Simon
  ```

2. 普通声明函数：`this`指向`window`或`undefined`（严格模式）
3. 在`new`中调用
  - 使用`new`时，`this`的改变过程如下：
    - 创建`this={}`
    - `new`执行的过程中可能改变`this`，然后添加属性和方法
    - 返还被改变的`this`
  - 如果构造函数返回一个对象，那么`this`也会指向这个对象
  ```js
  function Animal1(name) {
    this.name = name;
    this.canWalk = true;
  }
  function Animal2() {
    this.name = 'Mousie';
    this.age = '18';
    return {
      name: 'Godzilla'
    }
  }

  var animal1 = new Animal1('beastie');
  console.log(animal1.name);  // => beastie

  var animal2 = new Animal2();
  console.log(animal2.name);  // => Godzilla
  console.log(animal2.age);   // => undefined
  ```

4. 使用`call`或`apply`：通过第一个参数绑定执行上下文
5. 使用`bind`
  - `bind`不同于`call`和`apply`，它不立刻执行函数，而是返回一个绑定好执行上下文的新函数
  - `bind`创建了一个永恒的上下文链并不可修改，即不可再重新绑定
6. 箭头函数
  - 箭头函数的执行上下文是静态的，不会因为不同的调用而不同
  - 函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象

### 参考资料
[不再彷徨，JavaScript中this完全笔记（译文综合）](http://www.jianshu.com/p/b77430fefbd5)
[ES6函数的扩展](http://es6.ruanyifeng.com/#docs/function#箭头函数)
