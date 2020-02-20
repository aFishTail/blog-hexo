---
title: javascript高级程序设计之面向对象的程序设计
categories: 学习笔记
tags: Javascript
---
**学习目标**
- 理解对象属性
- 理解并创建对象
- 理解继承
>ECMA-262把对象定义为：“无序属性的集合，其属性可以包含基本值、对象或者函数。
## 理解对象  
- 创建自定义对象的最简单方式是创建一个Object实例，然后为它添加属性和方法,早期一般采用这种方法
```
var person = new Object()
person.name = 'tangyuan'
person.age = 2
person.job = 'Software Engineer'
person.sayName = function(){
    console.log(this.name)
}
```
- 几年后，对象字面量成为创建这种对象的首选
```
var person = {
    name: 'tangyuan',
    age: 29,
    job: 'Software Engineer'

    sayName: function(){
        console.log(this.name)
    }
}
```
### 属性类型
#### 数据属性
1. **Configurable** 表示能否通过delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为true
2. **Enumerable** 表示能否通过**for in**循环返回属性
3. **Writable** 表示能否修改属性的值
4. **Value** 包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。这个特性的默认值为undefined  
>对于像前面例子中那样直接在对象上定义的属性，它们的[[Configurable]]、[[Enumerable]]
和[[Writable]]特性都被设置为 true，而[[Value]]特性被设置为指定的值。例如：
var person = { 
 name: "Nicholas" 
};
##### 修改属性的方法：Object.defineProperty()  
这个方法接收三个参数：
   - 属性所在的对象
   - 属性的名字
   - 描述符对象，其属性必须是configurable、enumerable、writable 和 value
    ```
    var person = {}
    Object.defineProperty(person,'name',{
        writable: false,
        value: 'Nicholas'
    })
    console.log(person.name) // Nicholas
    person.name = 'tangyuan'
    console.log(person.name) // Nicholas
    ```

  上例name属性是只读的，这个属性值是不可修改的，非严格模式下，赋值操作将被忽略，严格模式下，赋值操作将会导致错误

  把configurable设置为false，表示不能从对象中删除属性。如果对这个值调用delete，非严格模式下正常，严格模式下将会报错
  ```
  var person = {}; 
  Object.defineProperty(person, "name", { 
  configurable: false, 
  value: "Nicholas" 
  })
  //抛出错误
  Object.defineProperty(person, "name", { 
   configurable: true, 
  value: "Nicholas" 
  })
  ```
#### 访问器属性

访问器属性不包含数据值；它们包含一对儿 getter 和setter 函数（不过，这两个函数都不是必需的）。
在读取访问器属性时，会调用 getter 函数，这个函负责返回有效的值；在写入访问器属性时，会调用
setter 函数并传入新值，这个函数负责决定如何处理据

  - **configurable**
  - **Enumerable**
  - **Get**
  - **Set**

访问器属性不能直接定义，必须使用Object.defineProperty()定义
```
var book = {
    _year: 2004,
    edition: 1
};
Object.defineProperty(book, "year", {
    get: function () {
        return this._year;
    },
    set: function (newValue) {
        if (newValue > 2004) {
            this._year = newValue;
            this.edition += newValue - 2004;
        }
    }
});
book.year = 2005;
console.log(book.edition); //2
```
### 定义多个属性
### 读取属性的特性
## 创建对象
### 工厂模式
### 构造函数模式
### 原型模式
#### 原生对象的原型  
原型模式的重要性不仅体现在创建自定义类型方面，就连所有原生的引用类型，都是采用这种模式创建的

通过原生对象的原型，不仅可以取得所有默认方法的引用，而且也可以定义新方法。可以像修改自定义对象的原型一样修改原生对象的原型
#### 原型对象的问题  
私有属性不适合共享
### 组合使用构造函数  
创建自定义类型最常见的方式，就是组合使用构造函数模式与原型模式。构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性
### 动态原型模式  
把所有信息都封装在了构造函数中，而通过在构造函数中初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的优点
```
function Person(name, age, job) {
    //属性
    this.name = name;
    this.age = age;
    this.job = job;
    //方法
    if (typeof this.sayName != "function") {
        Person.prototype.sayName = function () {
            console.log(this.name);
        };
    }
}

var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();
```
### 寄生构造函数模式
### 稳妥构造函数模式
## 继承
### 原型链
### 借用构造函数模式
### 组合继承
### 原型式继承
### 寄生式继承
### 寄生组合式继承




