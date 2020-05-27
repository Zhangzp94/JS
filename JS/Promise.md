### 1、Promise准备知识 

##### 01 函数对象和实例对象

(  )左边是函数对象， . 左边是实例对象

![01](..\images\b_images\01.png)



##### 02 回调函数的分类

![2](..\images\b_images\2.png)



##### 03 error错误类型

![3](..\images\b_images\3.png)



### 2、Promise的理解和使用

##### 01 promise是什么？

***1、定义***

语法上：Promise是一个构造函数

功能上：promise对象用来**封装**一个*异步操作*并可以**获取**其*结果*



##### 02 promise流程



![4](..\images\b_images\4.png)



##### 03 为什么使用promise？

面试的时候就说下面的！

回调地狱就是 函数嵌套，并且回调函数运行要建立在前面的函数完成的情况！

![5](..\images\b_images\5.png)

***解释第一点为啥更加灵活***

```js
function promise() {
    return new Promise((resolve, reject)=>{
        let num = true;
        if (num){
            resolve()
        }else{
            reject()
        }
    })
}
function successCallback(res){
    console.log(res)
}
function failedCallback(error){
    console.log(error)
}
setTimeout(()=>{
    //在2秒之后才指定的回调函数，得到结果的数据！更方便
    //总的来说就是时间上更加的灵活！！！
    promise().then(successCallback,failedCallback)
},2000)
//下面是传统的异步
//一开始就需要定义好回调函数！
promise(fn,successCallback,failedCallback)
```

```js
//promise链式调用
promise().then(function (res) {
    return promise(res)
}).then(function (resTwo) {
    return promise(resTwo)
}).then(function (resEnd) {
    return promise(resEnd)
}).catch(failedCallback)
```

***解决回调地狱终极方案！！！***

```js
//async / await 回调地狱的解决方案
async function req() {
  try {
      const one = await doOne()
      const two = await doTow(one)
      const end = await doEnd(two)
      console.log(end)
  }catch (error) {
      console.log(error)
  }
}
```



##### 04 Promise其他的API

***01 Promise.resolve( )***

它是一个语法糖

```js
//Promise语法糖
//产生一个成功值为1的promise对象
const a = Promise.resolve(1)
a.then((value)=>{
    console.log(value)
})
```

***02 Promise.reject( )***

```js
//Promise语法糖
const a = Promise.resolve(1)
const b = Promise.resolve(2)
const c = Promise.reject(3)
a.then((value)=>{
    console.log(value)//2
})
c.then(null,fail=>{
    console.log(fail)//3
})
```

***03 Promise.all( )***

all 的参数是数组，只要有一个reject就失败！

```js
const a = Promise.resolve(1)
const b = Promise.resolve(2)
const c = Promise.reject(3)

Promise.all([a,b,c]).then((values)=>{
    console.log(values);//去掉c 就是[1,2]
},(onRejected)=>{
    console.log(onRejected) //3
})
```

***04 Promise.race( )***

```js
const a = Promise.resolve(1)
const b = Promise.resolve(2)
const c = Promise.reject(3)

//从开头开始，质押碰到第一个成功的，就结束！
Promise.race([a,b,c]).then((values)=>{
    console.log(values);//1
},(onRejected)=>{
    console.log(onRejected)
})
```

##### 05 改变Promise的状态



![6](..\images\b_images\6.png)![7](..\images\b_images\7.png)



##### 06 Promise.then返回新的promise

**问题：**promise.then( ) 返回的新promise的结果状态由什么决定？

promise中.then返回新的promise对象，由前面.then()指定的回调函数执行的结果决定（就是看回调return 什么），

![8](..\images\b_images\8.png)



##### 07 promise内创建异步任务

下面第三种就是在promise内创建异步任务！

举一反三：就可以用promise串联 同步和异步操作！

如果是异步就是在promise内 包裹一个定时器即可！

![9](..\images\b_images\9.png)



##### 08 Promise穿透和中断



![10](..\images\b_images\10.png)

```js
new Promise((resolve, reject)=>{
    reject(1)
}).then(
    (value)=>{
        console.log(value)
    } ,
    (error)=>{
        console.log(error) //1
        throw error  //这里抛出错误 下面就是error回调执行！
    }
).then(
    (value)=>{
        console.log(value)
    } ,
    (error)=>{
        console.log(error)
       // throw error //这里再次抛出错误 知道catch执行！这就是穿透！
        return Promise.reject(error) //这种方法也可以，同上！
    }
).catch(
    (error)=>{
        console.log(error)
        //return Promise.reject(error) //在catch里面抛出错误 下面error回调函数执行！
        //return Promise.resolve(error) //下面value回调函数执行！
        return new Promise(()=>{}) //Promise中断！！这里处于pending状态！返回一个pending的Promise
    }
).then(
    (value)=>{
        console.log(value)
    } ,
    (error)=>{
        console.log(error)
    }
)
```



### 3、自定义(手写)Promise



````
为啥Promise是异步的呢?
比如new 的时候在里面发送ajax请求，那么有延迟，这时候是异步操作，

promise里面的resolve()是同步
.then()也是同步，这一步是根据用户调用的是resolve 还是 reject 去指定回调函数是谁，
.then()里面的回调函数是异步！因为.then指定好后，callbacks数组有了长度！
源码中的resolve才去执行这个回调！
所以源码中执行 then中的回调函数必须是异步！ then确定执行哪个回调-- 返回relove执行 ，之所以还返回是因为里面有异步等待！等待你指定哪个回调呢
````

