---
title: 使用Promise封装定时器实现轮询
tags:
---
```
let ocRes = false
setTimeout(()=>{
    ocRes = true
},5000) // 模拟5秒后OC出结果
/**
 * 
 * @param {*} interval 查询间隔
 * @param {*} timeOut  超时时间
 */
function checkOc(interval, timeOut) {
    return new Promise((resolve, reject) => {
        let timeConsum = 0
        const timer = setInterval(() => {
            timeConsum += interval
            if (ocRes) {
                clearInterval(timer)
                resolve(`调用成功,耗时：${timeConsum}`)
            } else {
                if (timeConsum > timeOut) {
                    clearInterval(timer)
                    reject(new Error('调用超时'))
                }
            }
        }, interval)
    })
}
checkOc(500, 6000).then(res => {
    console.log('正确', res)
}).catch(err => {
    console.log('错误', err)
})

```