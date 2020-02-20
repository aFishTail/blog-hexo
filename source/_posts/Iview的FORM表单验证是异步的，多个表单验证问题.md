---
title: Iview的FORM表单验证是异步的，多个表单验证问题
tags:
  - Javascript
  - Vue
  - Iview
  - ES6
date: 2020-02-20 09:30:04
---

Iview 的 Form对整个表单进行验证的方法是validate，官方是这样说的
>参数为检验完的回调，会返回一个 Boolean 表示成功与失败，支持 Promise

这里只是说了可以支持Promise,返回一个Boolean。那么返回值到底是不是异步的呢？
```
console.log('begin')
this.$refs['form'].forEach(ele => {
  ele.validate(valid => {
    console.log('返回校验值', valid)
  })
})
console.log('end')
// 返回结果是
// begin
// end
// 返回校验值, false
```

可以看出，返回是异步的。既然是异步的，那就处理异步就OK了。那就直接掏出异步终极解决方案，**async**,**await**

```
console.log('begin')
this.$refs['form'].forEach(async ele => {
  const valid = await ele.validate()
  console.log('返回校验值', valid)
})
console.log('end')
// 返回结果依然是
// begin
// end
// 返回校验值, false
```

这是为什么呢，都处理异步了，怎么还是先打印出end了，搜了一番资料才发现，forEach本身是同步的，如果回调函数是异步的，会直接放入调用栈，结束遍历。光解释可能不太清楚，上代码：  

```
[1, 2, 3].forEach((value) => {
    setTimeout(function() {
        some code;
    }, 1000);
});
other code; // 这部分代码不会被setTimeout阻塞，forEach遍历完1,2,3之后就执行

[1, 2, 3].forEach( async (value) => {
    let foo = await promiseFn();
});
other code; // 同样不会受到异步阻塞
```

而使用常规**for**循环就可以解决这个问题

```
const sleep = timer => {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, timer)
  })
}
const foo = async () => {
  for (let i =0 ; i < 5; i++) {
    await sleep(1000)
    console.log(i)
  }
}
foo()
//每隔1秒，依次输出0到4
```
参考文章：[js中forEach回调同异步问题](https://segmentfault.com/a/1190000018207183)
