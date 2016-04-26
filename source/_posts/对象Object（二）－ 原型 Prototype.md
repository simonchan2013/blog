title: 对象Object（二）－ 原型 Prototype
date: 2016-04-26 19:52:44
tags: javaScript Object
categories: javaScript笔记
---
# 对象Object（二）－ 原型 Prototype

### 原型模式
###### 原型对象
- 每新建一个函数（包括构造函数），都会创建一个指向函数**原型对象（原型）**的prototype属性。
- 默认情况下，函数对象会获得一个constructor属性，这个属性包含一个指向prototype属性所在函数的指针。
- 对象实现可以访问原型的值，但是无法修改原型的值。
- 在实例中添加一个属性（原型里已存在），则该属性会屏蔽原型中的同名属性，而不是覆盖

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

- 实例、原型、构造函数对应关系如下图：
<div style="text-align:center;"><img src="./images/obj_object_prototype_relation.png" style="width:780px;"></div>

- 可使用hasOwnProperty()方法检测一个属性是否存在于实例中（返回true表示存在于实例中）。in操作法则可以监测属性是否存在（不管在原型中还是实例中），两者配合则可以监测原型中的属性。

````javascript
console.log(person1.hasOwnProperty('name'));  // => false
person1.name = 'Grey';
console.log(person1.hasOwnProperty('name'));  // => true
delete person1.name;
console.log(person1.hasOwnProperty('name'));  // => false

// 结合使用hasOwnProperty()方法和in操作符就可以确定属性是存在于对象实例中还是存在于原型中
function isPrototypeProperty(obj, attrName) {
  return !obj.hasOwnProperty(attrName) && (attrName in obj);
}

var person = new Person();
console.log(isPrototypeProperty(person, 'name')); // => true
person.name = 'Grey';
console.log(isPrototypeProperty(person, 'name')); // => false
````

- 重写原型创建对象时，需要把constructor重新指回函数本身

````javascript
function Person() {}

Person.prototype = {
  constructor: Person,  // 会被设置成可枚举
  name: 'Nicholas',
  age: 29,
  job: 'Software Engineer',
  sayName: function() { console.log(this.name); }
}
// ECMAScript5兼容的情况下可用以下方法重设constructor
// Object.defineProperty(Person.prototype, 'constructor', {
//   enumerable: false,
//   value: Person
// });

var person = new Person();
console.log(person instanceof Object);      // => true
console.log(person instanceof Person);      // => true
console.log(person.constructor == Person);  // => true
console.log(person.constructor == Object);  // => false
````

- 在使用构造函数创建实例后，再重写原型对象，则prototype中的方法对该实例不生效。原因在于重写原型相当于切断了实例与原型之间的联系，关系如下图：
<div style="text-align:center;"><img src="./images/obj_instance_prototype_relation.png" style="width:500px;"></div>

````javascript
function Person() {}

var person = new Person();

// Person.prototype.sayHi = function() {
//   console.log('hi');
// }
// person.sayHi();   // => hi    // It's OK

Person.prototype = {
  constructor: Person,
  name: 'Nicholas',
  age: 29,
  job: 'Software Engineer',
  sayName: function() { console.log(this.name); }
}

// person.sayName();   // => TypeError: person.sayName is not a function
try {
  person.sayName();
}
catch(e) {
  console.log(e);     // => [TypeError: undefined is not a function]
}

var person2 = new Person();
person2.sayName();    // => Nicholas
````

- 原型对象的问题：导致所有实例公用引用类型值

````javascript
// 原型模式的问题，共用了引用类型值的属性
function Person() {}
Person.prototype = {
  constructor: Person,
  name: 'Nicholas',
  age: 29,
  job: 'Software Engineer',
  friends: ['Shelby', 'Court'],
  sayName: function() { console.log(this.name); }
}

var person1 = new Person();
var person2 = new Person();

person1.friends.push('Van');

console.log(person1.friends); // => [ 'Shelby', 'Court', 'Van' ]
console.log(person2.friends); // => [ 'Shelby', 'Court', 'Van' ]
console.log(person1.friends === person2.friends); // => true
````


&nbsp;
### 其他创建对象方法

````javascript
// 组合使用构造函数模式和原型模式 － 使用最广泛
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = ['Shelby', 'Court'];
}

Person.prototype = {
  constructype: Person,
  sayName: function() { console.log(this.name); }
}
````

````javascript
// 动态原型模式
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = ['Shelby', 'Court'];

  // functions
  if (typeof this.sayName != 'function') {
    Person.prototype.sayName = function() {
      consol.log(this.name);
    }
  }
}
````

````javascript
// 寄生构造函数 － 类似工厂模式
function Person(name, age, job) {
  var o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function() {
    console.log(this.name);
  };

  return o;
}
````
