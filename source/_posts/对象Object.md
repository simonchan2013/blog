title: 对象Object
date: 2016-03-25 01:20:11
tags: javaScript Object
categories: javaScript笔记
---
# Object
### 两种属性（property）：数据属性和访问器属性
#### 数据属性的四个特性（attribute）
- [[Configurable]]：能否重新设置属性（不是属性的值），包括delete、修改特性或者修改为访问器属性。直接定义在Object中的属性的这个特性默认为true
- [[Enumerable]]：表示能否通过for-in循环返回属性。默认为true
- [[Writable]]：表示能否修改属性的值。默认为true
- [[Value]]：该属性的数据值。默认为undefined

**在调用Object.defineProperty()方法设置特性时，如果不指定，configurable、enumerable和writable默认值都是false**
````javascript
var person1 = {
  name: 'Simon'
}
console.log(person1.name);  // => Simon
person1.name = 'csm';
console.log(person1.name);  // => csm
delete person1.name;
console.log(person1.name);  // => undefined


var person2 = {};
Object.defineProperty(person2, 'name', {
  configurable: false,
  writable: false,
  value: 'Simon'
});
console.log(person2.name);  // => Simon
person1.name = 'csm';
console.log(person2.name);  // => Simon
delete person2.name; // 严格模式下会报错
console.log(person2.name);  // => Simon


/*
  一旦设置属性为不可配置之后(configurable设为false)，就不能再将属性设置为可配置的。
  此时再调用Object.defineProperty()方法修改的都会报错
*/
var person3 = {};
Object.defineProperty(person3, 'name', {
  configurable: false,
  value: 'Simon'
});

// console.log(person3.name);  // => Simon
// Object.defineProperty(person3, 'name', { configurable: true }); // => TypeError: Cannot redefine property: name
// Object.defineProperty(person3, 'name', { enumerable: true }); // => TypeError: Cannot redefine property: name
// Object.defineProperty(person3, 'name', { value: 'csm' }); // => TypeError: Cannot redefine property: name
// Object.defineProperty(person3, 'name', { writable: true }); // => TypeError: Cannot redefine property: name
````



#### 访问器属性的四个特性（attribute）
- [[Configurable]]：能否重新设置属性，包括delete、修改特性或者修改为数据属性。直接定义在Object中的属性的这个特性默认为true
- [[Enumerable]]：表示能否通过for-in循环返回属性。默认为true
- [[Get]]：读取属性时调用的函数。默认为undefined
- [[Set]]：写入属性时调用的函数。默认为undefined

**访问器属性不能直接定义，必须使用Object.defineProperty()来定义**
````javascript
var book = {
  _year: 2004,
  edition: 1
};

Object.defineProperty(book, 'year', {
  get: function() {
    return this._year;
  },
  set: function(newValue) {
    if (newValue > 2004) {
      this._year = newValue;
      this.edition = newValue - 2004 + 1;
    }
  }
});

book.year = 2016;
console.info(book); // => { _year: 2016, edition: 13 }



// 定义多个属性 => Object.defineProperties()，部分浏览器支持
var book = {};

Object.defineProperties(book, {
  _year: {    // 数据属性
    value: 2004,
    writable: true
  },
  edition: {  // 数据属性
    value: 1,
    writable: true
  },
  year: {     // 访问器属性
    get: function() { return this._year; },
    set: function(newValue) {
      if (newValue > 2004) {
        this._year = newValue;
        this.edition = newValue - 2004 + 1;
      }
    }
  }
});

console.info(book); // => { _year: 2004, edition: 1 }
book.year = 2016;
console.info(book); // => { _year: 2016, edition: 13 }
````





## 创建对象
##### 工厂模式：可以创建多个相似对象，但没有解决对象识别（即对象的类型）
##### 构造函数模式：可以识别对象类型，方法不能共用，每一个实例都有自己的方法实例。
````javascript
// 工厂模式
function createPerson(name, age, job) {
  var o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function() {
    console.log(this.name);
  }

  return o;
}

var person1 = createPerson('Nicholas', 29, 'Software Engineer');
var person2 = createPerson('Grey', 27, 'Doctor');

console.info(person1);  // => { name: 'Nicholas', age: 29, job: 'Software Engineer', sayName: [Function] }
person1.sayName();      // => Nicholas
console.info(person2);  // => { name: 'Grey', age: 27, job: 'Doctor', sayName: [Function] }
person2.sayName();      // => Grey



// 构造函数模式
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function() {
    console.log(this.name);
  };
}

var person1 = new Person('Nicholas', 29, 'Software Engineer');
var person2 = new Person('Grey', 27, 'Doctor');

console.info(person1);  // => { name: 'Nicholas', age: 29, job: 'Software Engineer', sayName: [Function] }
person1.sayName();      // => Nicholas
console.info(person2);  // => { name: 'Grey', age: 27, job: 'Doctor', sayName: [Function] }
person2.sayName();      // => Grey
// 可以识别对象类型
console.log(person1.constructor == Person); // => true
console.log(person2.constructor == Person); // => true
// 既是Person的实例，也是Object的实例
console.log(person1 instanceof Object); // => true
console.log(person1 instanceof Person); // => true
console.log(person2 instanceof Object); // => true
console.log(person2 instanceof Person); // => true
````

#### 原型模式
###### 原型对象
- 每新建一个函数（包括构造函数），都会创建一个指向函数**原型对象（原型）**的prototype属性。
- 默认情况下，函数对象会获得一个constructor属性，这个属性包含一个指向prototype属性所在函数的指针。
- 对象实现可以访问原型的值，但是无法修改原型的值。

````javascript
// 原型模式
function Person() {}

Person.prototype.name = 'Nicholas';
Person.prototype.age = 29;
Person.prototype.job = 'Software Engineer';
Person.prototype.sayName = function() { console.log(this.name); }

var person1 = new Person();
person1.sayName(); // => Nicholas
var person2 = new Person();
person2.sayName(); // => Nicholas
person1.name = 'Grey';
person1.sayName(); // => Grey       // 来自实例
person2.sayName(); // => Nicholas   // 来自原型prototype，person1无法修改prototype的值
delete person1.name;
person1.sayName(); // => Nicholas   // 来自原型prototype

// 实例共享
console.log(person1.sayName == person2.sayName);  // => true

// 检测[[Prototype]]
console.log(Person.prototype.isPrototypeOf(person1)); // => true
console.log(Person.prototype.isPrototypeOf(person2)); // => true

console.log(Object.getPrototypeOf(person1) == Person.prototype); // => true
console.log(Object.getPrototypeOf(person1).name); // => Nicholas // Person.prototype中的name
console.log(Object.getPrototypeOf(person2).name); // => Nicholas
````
![Object Prototype Relation Sample](http://127.0.0.1:3000/images/javaScriptObject/object_prototype_relation.png)
