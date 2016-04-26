title: 对象Object（一）
date: 2016-04-26 19:50:06
tags: javaScript Object
categories: javaScript笔记
---
# 对象Object（一）
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
