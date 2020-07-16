---
title: 快速搞定javascript前端面试学习笔记
date: 2020-07-16 10:42:38
tags: javascript 笔记
---
```
Date 2020/07/12
来源： 慕课网-快速搞定前端初级javascript面试题
```
# 快速搞定前端初级javascript面试题
## 一、简介
### 如何搞定所有面试题

### 前端知识体系

#### 什么是知识体系
##### 高效学习三部曲：
  - 找准知识体系
  - 刻意练习
  - 及时反馈

##### 知识体系：结构化知识范围
  
##### 涵盖所有知识点：
  - 结构化
  - 有组织
  - 易扩展

#### 从哪方面梳理
##### W3C标准
##### ECMA 262 标准
##### 开发环境 
##### 运行环境  

#### 知识体系目录

## 二、变量类型和计算
### 变量类型和计算
#### 题目
- typeOf能判断哪些类型
- 何时使用 === 何时使用 ==
- 值类型和引用类型的区别
- 手写深拷贝
#### 知识点
- 变量类型
  - 值类型 vs 引用类型
  - typeOf运算符
  - 深拷贝
- 变量计算
#### 解答

### typeof 和深拷贝
#### typeOf运算符
- 识别所有值类型
  ```
  let a; typeOf a // 'undefined'
  const str = 'abc' typeOf str // 'string'
  const n = 100 typeOf n // 'number'
  const b = true typeOf b // 'boolean'
  const s = Symbol('s') typeOf s // 'symbol'
  ```
- 识别函数
  ```
  typeOf console.log // 'function'
  typeOf function () {} // 'function'
  ```
- 判断是否是引用类型（不可再细分）
  ```
  typeOf null 'Object'
  typeOf ['a', 'b'] 'Object'
  typeOf {x: 100} 'Object'
  ```
#### 深拷贝
```
/**
 * 深拷贝
 * @param {Object} obj 要拷贝的对象
 */
function deepClone(obj = {}) {
    // 非复杂对象类型直接返回
    if(typeof obj !== 'object' || obj == null) {
        return obj
    }
    // 初始化返回结果
    let result
    if(obj instanceof Array) {
        result = []
    } else {
        result = {}
    }

    for(let key in obj) {
        if(obj.hasOwnProperty(key)) {
            // 保证key不是原型的属性
            result[key] = deepClone(obj[key])
        }
    }

    // 返回结果
    return result
}
```
### 变量计算
#### 类型转换
- 字符串拼接
  ```
  const a = 100 + 10 // 110
  const a = 100 + '10' // '10010'
  const a = true + '10' // 'true10'
  ```
- `==`
  ```
  100 == '100' // true
  0 == '' // true
  0 == false // true
  false == '' // true
  null == undefined // true
  ```
  `==`的隐式转化比较复杂，目前实际应用意义较小，没必要深究
  建议除了 `==` null 之外，其他一律使用 `===`
  ```
  const obj = {x: 100}
  if (obj.a == null) {}
  // 相当于
  // if(obj.a === undefined || obj.a === null) {}
  ```
- if语句和逻辑运算
  - truly 变量： `!!a === true`
  - falsely 变量 `!!a === false`
  ```
  !!0 === false
  !!NaN === false
  !!'' === false
  !!null === false
  !!undefined === false
  !!false === false
  ```
- 逻辑判断
  ```
  console.log(10 && 0) // 0
  console.log('' || 'abc') // 'abc'
  console.log(!window.a) // true
  ```
## 三、原型和原型链
### class
```
  // 类
  class Student {
      constructor(name, number) {
          this.name = name
          this.number = number
      }
      sayHi() {
          console.log(`姓名：${this.name},学号：${this.number}`)
      }
  }

  // 通过new来生成对象/实例
  const xialuo = new Student('夏洛', 100)
  console.log(xialuo.name)
  console.log(xialuo.number)
  console.log(xialuo.sayHi())

  const madongmei = new Student('马冬梅', 101)
  console.log(madongmei.name)
  console.log(madongmei.number)
  console.log(madongmei.sayHi())
```
### 继承
```
  // 父类
  class Person {
    constructor(name) {
      this.name = name
    }
    eat() {
      console.log(`${this.name} eat something`)
    }
  }

  // 子类
  class Student extends Person {
    constructor(name, number) {
      super(name)
      this.number = number
    }
    sayHi() {
      console.log(`姓名：${this.name},学号：${this.number}`)
    }
  }
  // 另一个子类
  class Teacher extends Person {
    constructor(name, major) {
      super(name)
      this.major = major
    }
    teach() {
      console.log(`${this.name}教授：${this.major}`)
    }
  }

  const xialuo = new Student('夏洛', 100)
  console.log(xialuo.name)
  console.log(xialuo.number)
  xialuo.sayHi()
  xialuo.eat()

  const wanglaoshi = new Teacher('王老师', '数学')
  console.log(wanglaoshi.name)
  console.log(wanglaoshi.major)
  wanglaoshi.teach()
  wanglaoshi.eat()
```
### 类型判断 - instanceOf
```
xialuo intanceOf Student // true
xialuo intanceOf Person // true
xialuo intanceOf Object // true

[] instanceOf Array // true
[] instanceOf Object // true

{} instanceOf Object // true
```
### 原型
```
// class实际上是函数，可见是语法糖
typeOf Person // 'function'
typeOf Student // 'function'

// 隐式原型和显示原型
console.log(xialuo.__proto__)
console.log(Student.prototype)
console.log(xialuo.__proto__ === Student.prototype) // true
```
![原型.png](http://yanxuan.nosdn.127.net/63de8aa846192792f6e9aec2202e30a5.png)
#### 原型关系
- 每个`class`都有显式原型
- 每个实例都有隐式原型 `__proto__`
- 实例的`__proto__`指向对应`class`的`prototype`
#### 基于原型的执行规则
- 获取属性`xialuo.name` 或执行方法`xialuo.sayHi()`时
- 现在自身属性或方法寻找
- 如果找不到则自动去`__proto__`寻找

### 原型链
```
console.log(Student.prototype.__proto__)
console.log(Person.prototype)
console.log(Student.prototype.__proto__ === Person.prototype) // true
```
![原型链.png](http://yanxuan.nosdn.127.net/bfe43487fbcbc810032d14c685150b18.png)
#### 题目解答
- 如何准确判断一个变量是不是数组？
  `a instanceOf Array`
- 手写一个简易的jQuery, 考虑插件和扩展机智
  
## 三、作用域和闭包
### 题目
- this的不同应用场景，如何取值？
- 手写bind函数
  ```
  // 手写bind

  // 使用bind
  function fn1(a, b, c) {
      console.log('this', this)
      console.log(a, b, c)
      return 'this is this'
  }

  // const fn2 = fn1.bind({x: 100}, 1, 2, 3)
  // fn2()

  Function.prototype.bind1 = function () {
      const args = Array.from(arguments)
      const t = args.shift()
      const self = this
      return function () {
          self.apply(t, args)
      }
  }
  const fn2 = fn1.bind1({x: 100}, 1, 2, 3)
  fn2()
  ```
- 实际开发中闭包的应用场景，举例说明
  ```
    // 闭包实现缓存
  function createCache() {
      const data = {}
      return {
          get: function (key) {
              return data[key]
          },
          set: function(key, val) {
              data[key] = val
          }
      }
  }
  const c = createCache()
  c.set('name', 100)
  console.log(c.get('name'))
  ```
- 创建10个`<a>`标签，点击时弹出对应的序号
  ```
  let a, i
  for(i=0; i<10 ;i++) {
    document.createElement('a')
    a.innerHTML = i + '<br>'
    a.addEventListener('click', function(e) {
      e.preventDefault()
      alert(i)
    })
    document.body.appendChild(a)
  }
  // 每次点击都会弹出10

  // 下面这个才是正确的写法
  // let会生成块级作用域
  let a
   for(let i=0; i<10 ;i++) {
    a = document.createElement('a')
    a.innerHTML = i + '<br>'
    a.addEventListener('click', function(e) {
      e.preventDefault()
      alert(i)
    })
    document.body.appendChild(a)
  }
  ```
### 知识点
- 作用域和自由变量
- 闭包
  ```
  // 闭包

  // 函数作为返回值
  // function create() {
  //     let a = 100
  //     return function () {
  //         console.log(a)
  //     }
  // }
  // let fn = create()
  // let a = 200
  // fn() // 100

  // 函数作为参数
  function print(fn) {
    let a = 200
    fn()
  }
  let a = 100
  function fn() {
    console.log(a)
  }
  print(fn)

  // 闭包：自由变量的查找，是函数定义的地方，向上级作用域查找，而不是执行的地方
  ```
- this
  - 在class方法中调用
  - 箭头函数
  - 谁调用，this指向谁
```
function fn1() {
  console.log(this)
}
fn1() // window
fn1.call({x: 100}) // {x: 100}
const fn2 = fn1.bind({x: 200})
fn2() // {x: 200}
```

```
const zhangsan = {
  name: '张三',
  sayHi () {
    // this即当前对象
    console.log(this)
  },
  wait () {
    setTimeout(function () {
      // this === window
      console.log(this)
    })
  }
}

// 箭头函数不改变this指向
const zhangsan = {
  name: '张三',
  sayHi () {
    // this即当前对象
    console.log(this)
  },
  wait () {
    setTimeout(() => {
      // this即当前对象
      console.log(this)
    })
  }
}
```

在类中使用
```
class Person {
  constructor(name) {
    this.name = name
    this.age = age
  }
  sayHi() {
    console.log(this)
  }
}
const zhangsan = new Person()
zhangsan.sayHi() //zhangsan对象
```

## 五、异步和单线程
### 同步和异步的区别
#### 单线程和异步
- **JS**只是单线程语言，只能同时做一件事
- 浏览器和**nodejs**已支持JS启动进程，如`Web Worker`
- **JS**和**DOM**渲染共用同一个线程，因为JS可修改DOM结构
- 遇到等待（网络请求，定时任务）不能卡住
- 需要异步
### 应用场景
- 网络请求
- 定时任务
### promise
```
function getData(url) {
  return new Promise((resolve, reject) => {
    $.ajax({
      url,
      success(data) {
        resolve(data)
      },
      error(err) {
        reject(err)
      }
    })
  })
}
```
### 问题解答和总结
- 同步和异步的区别是什么
  - 基于JS是单线程语言
  - 异步不会阻塞代码执行
  - 同步会阻塞代码执行
- 手写用`promise`加载一张图片
  ```
  const url = "http://yanxuan.nosdn.127.net/63de8aa846192792f6e9aec2202e30a5.png"
  function loadImg(src) {
      return new Promise((resolve, reject) => {
          const img = document.createElement('img')
          img.onload = () => {
              resolve(img)
          }
          img.onerror = () => {
              reject(new Error('图片加载失败'))
          }
          img.src = src
      })
  }
  loadImg(url).then(img => {
      console.log('图片加载成功', img)
  }).catch(err  => {
      console.log(`失败原因: ${err}`)
  })
  ```
- 前端使用异步的场景有哪些

## 六、从JS基础知识到JS Web API
#### DOM节点的property
```
const pList = document.querySelectorAll('p')
p1 = pList[0]
p1.style.width = '100px'
console.log(p1.style.width)
p1.className = 'red'
console.log(p1.className)
console.log(p1.nodeName)
console.log(p1.nodeType)
```
#### DOM节点的attribute
```
p1.setAttribute('data-name', 'tangyuan')
console.log(p1.getAttribute('data-name')) // tangyuan
p1.setAttribute('style', 'font-size: 50px')
console.log(p1.style.fontSize) // 50px
```
#### DOM结构操作
- 新建节点
  ```
  const p1 = document.createElement('p')
  p1.innerHTML = 'this is p1'
  div1.appendChild(p1)
  ```
- 移动节点
  ```
  const p2 = document.getElementById('p2')
  div2.appendChild(p2)
  ```
  **添加已存在的节点会进行移动**
- 获取父元素
  `child.parentNode`
- 获取子元素
  `parent.childNodes`
- 删除节点
  `removeChild`
### DOM性能
- DOM操作非常‘昂贵’,避免频繁的DOM操作
- 对DOM查询做缓存
  ```
  // 不缓存 DOM 查询结果
  for(let i =0 ;i<document.getElementsByTagName('p').length; i++) {
    // 每次循环，都会计算length, 频繁进行DOM查询
  }

  // 缓存 DOM 查询结果
  const pList = document.getElementsByTagName('p')
  const length = pList.length
  for(let i=0; i<length; i++) {
    // 缓存DOM， 只进行一次DOM查询
  }
  ```
- 将频繁操作改为一次性操作
  ```
  const list = document.getElementById('list')

  // 创建一个文档片段，此时还没有插入到 DOM 结构中
  const frag = document.createDocumentFragment()
  for(let i=0; i<20; i++) {
      const li = document.createElement('li')
      li.innerHTML = `List Item ${i}`
      
      //先插入到文档片段
      frag.appendChild(li)
  }

  // 都完成之和，再统一插入到 DOM 结构中
  list.appendChild(frag)
  ```
## 七、BOM 操作
- navigator
  ```
  const ua = navigator.userAgent
  const isChrome = ua.indexOf('Chrome')
  console.log(isChrome)
  ```
- screen
  ```
  screen.width
  screen.height
  ```
- location
  ```
  console.log(location.href)
  console.log(location.protocol)
  console.log(location.pathname)
  console.log(location.search)
  console.log(location.hash)
  ```
- history
  ```
  history.back()
  history.forward()
  ```
## 八、事件
### 事件绑定和事件冒泡
- 事件绑定
  ```
  const btn = document.getElementById('btn')
  btn.addEventListener('click', event => {
    console.log('clicked')
  })

  // 通用的绑定函数
  function bindEvent(elem, type, fn) {
    elem.addEventListener(type, fn)
  }
  const a = document.getElementById('link')
  bindEvent(a, 'click', e => {
    e.preventDefault() //阻止默认行为
    console.log('clicked')
  })
  ```
- 事件冒泡
  ```
  const p = document.getElementById('p')
  const body = document.body
  bindEvent(p, 'click', e=> {
    e.stopPropagation()
    console.log('激活)
  })
  bindEvent(body, 'click', e => {
    console.log('取消')
  })
  ```
### 事件代理
```
// 通过事件冒泡实现代理
// 一个支持事件代理的通用事件绑定函数
function bindEvent(elem, type, selector,fn) {
    if(fn == null ) {
      fn = selector
      selector = null
    }
    elem.addEventListener(type, e => {
      let target
      if(selector) {
        target = e.target
        if(target.matches(selector)) {
          fn.call(target, e)
        }
      } else {
        fn.call(target, e)
      }
    })
  }

const div = document.getElementById('div')
bindEvent(div, 'click', event => {
  event.preventDefault()
  const target = event.target // 获取触发事件的节点
  if(target.nodeName === 'A') {
    console.log(target.innerHtml)
  }
})

const a = document.getElementById('link')
bindEvent(a, 'click', e => {
  e.preventDefault() //阻止默认行为
  console.log('clicked')
})
```
## 九、ajax
### XMLHttpRequest
#### 手写一个ajax请求：
```
const xhr = new XMLHttpRequest()
xhr.open('GET', '/data/test.json', false)

xhr.onreadystatechange = function () {
    if(xhr.readyState === 4) {
        if(xhr.status === 200) {
            alert(xhr.responseText)
        }
    }
}
xhr.send()
```
#### xhr.readyState
- 0 -（未初始化）还没调用send()方法
- 1 - （载入）已调用send()方法，正在发送请求
- 2 - （载入完成）send()方法执行完成，已接收到全部响应内容
- 3 - (交互) 正在解析响应内容
- 4 - （完成）响应内容解析完成，可以在客户端调用
#### xhr.status
- 2xx - 表示请求成功，如200
- 3xx - 需要重定向， 浏览器直接跳转，如 301 302 304
- 2xx - 客户端请求错误，如404， 403
- 2xx - 服务端错误
### 同源策略和跨域
#### 跨域
- 什么是跨域（同源策略）
- JSONP
- CORS（服务端支持）
#### 同源策略
- ajax请求时，浏览器要求当前网页和server必须同源（安全）
- 同源：协议、域名、端口，三者必须一致
- 前端：http://a.com:8080/；server:https://b.com/api/xxx
#### 加载 **css** **js** 可无视同源策略
- `<img src=跨域的图片地址 />`
- `<link href=跨域的css地址 />`
- `<script src=跨域的js地址></script>`
- `<img /> 可用于统计打点， 可使用第三方统计服务`
- `<link /> <script> 可使用**CDN**,**CDN**一般都是外域`
- `<script> 可实现JSONP`

#### 跨域
- 所有的跨域，都必须经过server端配合
- 未经server端允许就实现跨域，说明浏览器有漏洞，危险信号
### JSONP和CORS
- JSNOP
  ```
  <script>
  window.callback = function (data) {
    // 跨域得到的信息
    console.log(data)
  }
  </script>
  <script src="jsonp.js"></script>

  //jsonp文件内容
  callback({
    name: "jsonp"
  })
  ```
- CORS - 服务器设置 http header
  ```
  response.setHeader("Access-Control-Allow-Origin", "http://localhost:8080")
  response.setHeader("Access-Control-Allow-Headers", "X-Request-With")
  response.setHeader("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS")
  ```
## 十、存储
### cookie
- 本身用于浏览器和server通讯
- 被“借用”到本地存储
- 可用`document.cookie = ""` 修改
### cookie的缺点
- 存储太小，最大4K
- http请求时需要发送到服务端，增加请求数据量
- 只能用`document.cookie = ""`修改，太过简陋

### localStorage 和 sessionStorage
- **HTML5**专门为存储而设计，最大可存5M
- **API**简单易用 **setItem** **getItem**
- 不会随着**http**请求被发送出去
- **localStorage**数据会永久存储，除非代码或手动删除
- **sessionStorage**数据只存在于当前会话，浏览器关闭则清空
- 一般用**localStorage**会更多一些
### 描述 cookie localStorage sessionStorage
- 容量
- **API**易用性
- 是否跟随**http**请求发送出去
## 十一、开发环境
### git
#### 常用git命令
- git add
- git checkout xxx
- git commit -m xxx
- git push origin master
- git pull origin master
- git branch
- git checkout -b xxx/git checkout xxx
- git merge xxx
### webpack 和 babel
- ES6 模块化，浏览器暂不支持
- ES6 语法，浏览器并不完全支持
- 压缩代码，整合代码，以提高网页加载速度
### webpack环境搭建
```
// webpack.config.js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    mode: 'development',
    entry: path.join(__dirname, 'src', 'index.js'),
    output: {
        filename: 'bundle.js',
        path: path.join(__dirname, 'dist')
    },
    plugins:[
        new HtmlWebpackPlugin({
            template: path.join(__dirname, 'src', 'index.html'),
            filename: 'index.html'
        })
    ],
    devServer:{
        port: 3000,
        contentBase: path.join(__dirname, 'dist')
    }
}
```
```
// package.json
"scripts": {
    "test": "",
    "build": "webpack",
    "dev": "webpack-dev-server"
  }
```
### webpack-babel
```
// 安装babel相关依赖
npm i @babel/core @babel/preset-env babel-loader -D
```
```
// 在根目录下创建.babelrc文件
{
    "presets":["@babel/preset-env"]
}
```
```
// 在webpack.config.js使用babel-loader
module: {
      rules: [
          {
              test: /\.js$/,
              loader: ['babel-loader'],
              include: path.join(__dirname, 'src'),
              exclude: /node_modules/
          }
      ]  
}
```
### webpack生产环境配置
```
// 新建一个webpack.prod.js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    mode: 'production',
    entry: path.join(__dirname, 'src', 'index.js'),
    output: {
        filename: 'bundle[contenthash].js',
        path: path.join(__dirname, 'dist')
    },
    plugins:[
        new HtmlWebpackPlugin({
            template: path.join(__dirname, 'src', 'index.html'),
            filename: 'index.html'
        })
    ],
    module: {
      rules: [
          {
              test: /\.js$/,
              loader: ['babel-loader'],
              include: path.join(__dirname, 'src'),
              exclude: /node_modules/
          }
      ]  
    }
}
```
```
// package.json
"scripts": {
    "test": "",
    "build": "webpack --config webpack.prod.js",
    "dev": "webpack-dev-server"
  }
```
### linux常用命令
- ls
- mkdir
- rm
- cd
- mv
- cp
- touch
- vi
- cat head tail
- grep 查找 `grep "babel" package.json`
## 运行环境
- 网页加载过程
- 性能优化
- 安全
### 页面加载过程
#### 题目
- 从输入url到渲染出整个页面的过程
- window.onload 和 DOMContentLoaded 的区别
#### 知识点
- 资源加载的形式
  - html文件
  - 媒体文件，如图片、视频等
  - javascript css
- 资源加载的过程
  - DNS解析：域名 -> IP地址
  - 浏览器根据IP地址向服务器发起http请求
  - 服务器处理http请求，并返回给浏览器
- 页面渲染的过程
  - 根据 HTML 代码生成 DOM Tree
  - 根据 CSS 生成 CSSOM
  - 将 DOM Tree 和 CSSOM 整合形成 Render Tree
  - 根据 Render Tree 渲染页面
  - 遇到 `<script>` 则暂停渲染，优先加载并执行JS代码，完成再继续， 一直到Render Tree渲染完成
#### 思考:
- 为何建议把 css 放到 head 中？
  答：先加载css，DOM tree生成之和，两者整合生成Render Tree，避免多次渲染
- 为什么把 js 放在body最后？
  答：加载`<script>`标签会暂停渲染
#### 问题解答
- 从输入url到渲染出整个页面的过程
  - 下载资源：各个类型的资源，下载过程
  - 渲染页面：结合 html css javascript 图片等
- window.onload 和 DOMContentLoaded 的区别
  - window.onload 资源全部加载完才能执行，包括图片
  - DOMContentLoaded DOM渲染完成即可，图片可能尚未下载

## 十三、性能优化
### 性能优化原则和方向
#### 性能优化原则
- 多使用内存、缓存或其他方法
- 减少CPU计算量，减少网络加载耗时
- 空间换时间
#### 从何入手
##### 让加载更快
- 减少资源体积：压缩代码
- 减少访问次数：合并代码，SSR 服务器端渲染，缓存
  **合并请求：**
  ```
  //多次请求
  <script src="a.js"></script>
  <script src="b.js"></script>
  <script src="c.js"></script>
  
  // 合别为一次请求
  <script src="abc.js"></script>
  ```
  **缓存:**
  ```
  // webpack打包时给文件名加上hash
  output:{
    filename: 'bundle.[contenthash].js',
    path: path.join(__dir, 'dist')
  }
  ```
  - 静态资源加hash后缀，根据文件内容计算hash
  - 文件内容不变，则hash不变，则url不变
  - url和文件不变，则会自动触发http缓存机制，返回304
  **SSR**
  - 服务端渲染： 将网页和数据一起加载，一起渲染
  - 非SSR(前后端分离)：先加载网页，再加载数据，再渲染数据
  - 最早的JSP ASP PHP, 现在的vue React SSR
- 使用更快的网络：CDN
##### 让渲染更快
- CSS放在head， JS 放在body最下面
- 尽早开始执行JS，在 `DOMContentLoaded`触发
  ```
  window.addEventListener('load', function(){
    // 页面全部资源加载完毕才会执行，包括图片、视频等
  })

  document.addEventListener('DOMContentLoaded', function(){
    // DOM 渲染完成即可执行，此时图片、视频还没有加载完
  })

  ```
- 懒加载(图片懒加载， 上滑加载更多)
  ```
  <img id="img" src="default.png" data-realsrc="abc.png">

  <script type="text/javascript">
  var img = document.getElementById('img')
  img.src = img.getAttribute('data-realsrc')
  </script>
  ```
- 对 DOM 查询进行缓存
- 避免频繁操作DOM，合并一起插入DOM结构
  使用`document.createDocumentFragment()`创建文档片段，合并DOM操作
- 节流 `throttle`  防抖 `debounce`
### 防抖
简易实现方式
```
const input = document.getElementById('input')
let timer = null
input.addEventListener('keyup', function (e) {
  if (timer) {
    clearTimeout(timer)
  }
  timer = setTimeout(() => {
    console.log(e.target.value)
    timer = null // 清除定时器
  }, 500)
})
// 每次输入之和隔500毫秒输出当前value
```
封装成debounce函数
```
function debounce(fn, delay) {
  let timer
  return function () {
    if (timer) {
      clearTimeout(timer)
    }
    timer = setTimeout(() => {
      fn.apply(this, arguments)
      timer = null
    }, delay)
  }
}
input.addEventListener(
  'keyup',
  debounce((e) => {
    console.log(e.target.value)
  }, 500)
)
```
### 节流
简单实现
```
const div = document.getElementById('div')
let timer
div.addEventListener('drag', function(e)  {    
    if(timer) return
    timer = setTimeout(() =>{
        console.log(e.offsetX, e.offsetY)
        timer = null
    }, 500)
})
```
封装成函数
```
function throttle(fn, delay) {
    let timer
    return function () {
        if(timer) return
        timer = setTimeout(() => {
            fn.apply(this, arguments)
            timer = null
        }, delay)
    }
}
div.addEventListener('drag', throttle((e) => {
    console.log(e.offsetX, e.offsetY)
}, 500))
```
十三、面试技巧
### 关于简历
- 简洁明了，突出个人技能和项目经验
- 可以把个人博客、开源作品放在简历上（但博客要有内容）
- 不要造假，保证能力上的真实性（斟酌用词，如精通xxx）
### 面试过程中注意事项
- 如何看待加班：像借钱，救急不救穷
- 千万不要挑战面试官，反考面试官
- 学会给面试官惊喜，证明你能想到更多，做的更多，但不要太多
- 遇到不会的问题，说出自己会的部分即可，但别岔开话题
- 谈谈你的缺点：说下最近在学什么。
## 十四、面试真题
#### 何为变量提升
- var是ES5语法，let const 是 ES6语法；var有变量提升
- var let是变量，可修改；const是常量，不可修改
- let const 有块级作用域；var 没有
#### typeOf 能判断哪些类型
- undefined string number boolean symbool
- object(注意， typeOf null === 'object')
- function
#### 列举强制类型转换与隐式类型转换
- 强制：`parseInt` `parseFloat` `toString`等
- 隐式：if、逻辑运算、==、+ 拼接字符串
#### 手写深度比较
#### split()和join()区别
```
'1-2-3'.split('-') // [1,2,3]
[1,2,3].join('-') // '1-2-3'
```
#### 数组的pop push unshift shift 分别做什么
#### 数组的API，有哪些是纯函数
> 纯函数：1. 不改变原数组（没有副作用）；2. 返回一个新数组
- concat
- map
- filter
- slice
`forEach some every`也不改变原数组，但是具有返回值
#### 数组slice和splice的区别
slice是纯函数， splice非纯函数
#### [10,20,30].map(parseInt)返回结果是什么
```
[10,20,30].map(parseInt)
// 可以转化为下面这种形式
[10,20,30].map((n,index) => {
  return parseInt(n, index)
})
```
#### ajax请求get 和 post 区别？
- get一般用于查询，post一般用于提交操作
- get参数拼接在url上，post放在请求体内
- 安全性：post易于防止CSRF
#### 函数call 和 apply 区别
```
fn.call(this, p1,p2,p3)
fn.apply(this, [p1,p2,p3])
```
#### 时间代理（委托）是什么
通过事件冒泡机制实现
#### 闭包是什么？有什么特性？有什么负面影响？
- 作用域和自由变量
- 应用场景：作为参数传入，作为返回值返回
- 自由变量的查找，在**函数定义的地方**
- 影响： 变量常驻内存，内存得不到释放
#### 如何阻止事件冒泡和默认行为？
- event.stopPropagation()
- event.preventDefault()
#### 查找、添加、删除、移动DOM节点的方法？
#### 如何减少DOM操作？
- 缓存DOM查询结果
- 多次DOM操作，合并到一次插入(createDocumentFragment)
#### 解释jsonp的原理，为何它不是真正的ajax？
- 浏览器的同源策略和跨域
- 哪些html标签能绕过跨域
- jsonp原理
#### document load 和 ready 的区别
- window.onload 资源全部加载完才能执行，包括图片
- DOMContentLoaded DOM渲染完成即可，图片可能尚未下载
#### == 和 === 的不同
- == 会尝试类型转换
- === 严格相等
- 建议只有 == null 这种情况下使用 ==
#### 函数声明和函数表达式的区别
- 函数声明 `function (){}`
- 函数表达式 `const fn = function(){}`
- 函数声明会在代码执行之前预加载，会变量提升，而函数表达式不会
#### new Object() 和 Object.create()的区别？
- {} 等同于new Object(), 原型Object.prototype
- Object.create(null) 没有原型
- Object.create({...}) 可指定原型
#### 关于this的使用场景
#### 手写字符串 trim 方法，保证浏览器兼容性
```
String.prototype.trim = function () {
  return this.replace(/^\s+/, '').replace(/\s+$/, '')
}
```
#### 如何获取多个数字中最大值
```
/**
 * 取数组中最大值
 * @param {number[]} nums  数组
 */
function max(nums) {
    // let arr = Array.from(arguments)
    let max = 0
    for(let value of nums) {
        max = Math.max(max, value)
    }
    return max
}
```
#### 如何用JS实现继承
- class
- prototype
#### 如何捕获JS程序中异常
- 
  ```
  try {

    } catch(err) {
      console.log(err)
    } 
  ```
- `window.onerror = function(){}`
#### 什么是JSON
- json是一种数据格式，本质是字符串
- json格式和JS对象结构一致，对JS语言更友好
- window.JSON是一个全局对象：JSON.stringify JSON.parse
#### 获取当前页面的url参数
- 传统方式，通过`location.search`
- 新API， `URLSearchParams`
#### 将url参数解析为JS对象
```
// 传统方式，分析search
function queryToObj() {
  const res = {}
  const search = location.search.substr(1) // 去掉前面的”？“
  search.split('&').forEach((p) => {
    const arr = p.split('=')
    const key = arr[0]
    const val = arr[1]
    res[key] = val
  })
  return res
}

// 使用URLSearchParams
function queryToObj() {
    const res = {}
    const pList = new URLSearchParams(location.search)
    pList.forEach((val, key) => {
        res[key] = val
    })
    return res
}
```
#### 手写数组flatern, 考虑多层级
#### 数组去重
#### 手写深拷贝
#### 介绍一下RAF requestAnimateFrame
#### 前端性能如何优化？一般从哪几方面进行考虑？