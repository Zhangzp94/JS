### 第三章   代码测试和错误处理

##### try-catch语句

```shell
语法：
try{
	//可能出现错误的代码
}catch(e){ //参数 e 是错误对象，对象里面有一些错误属性信息，e.message
	//在错误发生时怎么处理
}
exp:
try{
	a+b
}catch(e){
	alert(e.message)
}
```

##### finally语句

```shell
语法：
try{
	return 1
}catch(e){
	return 2
}finally{
	return 0
}
//加了finally语句，那么最后必须执行它，最终结果是 0
```

### 第四章 基础语法

#### 数据类型转换

##### 1.数值

```shell
var float = 1.2e3  等价于  var float = 1.2*10*10*10 

var float  = 0.1 + 0.2 ，错误写法，不能直接小数相加，可以先转化成整数再除10

Number.toString() //参数可以是2-36的数字，把数字转为任意进制的字符串
//1
var a = 32
a.toString(2) //输出100000 ；把32转为2进制的数 ， 参数范围2~36，
//2
data.toSting() //不传参数的时候，data对象 用于把读取的文件转为字符串的形式
//3
var color = 0x000000
'#'+color.toString(16) //转化成颜色
//4
3.toFied(2)//保留数值后2位，3.00 ,范围 0-100之间
parseFloat('3.145') // 把字符串转为数字
parseFloat('3.1a') // 3.1
parseInt('3.145') //输出 3
Number('3.2a')  //NaN





//判断一个值是否可以做数字的方法
isNaN --非数字 ，它是一个静态函数
isNaN(0) --false
isNaN('oop') --true
isNaN('0') --false
但是判断值是否可以做数字的方法最好用 isFinite函数，因为它可以筛除掉NaN 和无穷大的值
exp:
var isNumber = function isNumber(value){
	return typeof value === 'number' && isFinite(value)
}

补充：
Math.floor(20.5) //输出 20
Math.round(20.5) //输出 21
```

##### 严格模式

```shell
<script>
'use strict'
...
</script>
```

##### typeof

```shell
typeof检测返回都是 字符串
typeof null  --"object"
由于null的时候，返回的是object，所以可以写个函数判断
function type(a){
	return (a===null)?'null':typeof a ;
}
```

##### constructor 检测类型

对于数组、对象等复杂数据，可以使用constructor属性进行检测，该属性是构造器

因为typeof检测 [ ] 类型，返回是 “object” ，

```shell
var a = {}
var b = []
a.constructor == Object --返回true
b.constructor == Array  --返回true
```

##### toString() 方法

对象、数组、函数等都包含toString （）方法

##### 一元运算符

```she
var i = 1
1.变量在前面（贴近等号），先给赋值给左边，再自己计算
var s = i++ ; // (s,i)--(1,2)
2.变量在后面，先给自己计算，再赋值
var s = ++i ; // (s,i) -- (2,2)
```

# JS基础

```js
1.Math对象
2.date
3.string
4.arrary

```



##### 删除对象属性

```js
1.delete 属性！

fucntion Foo(){
    this.age = 20;
}
var foo = new Foo()
delete foo.age
console.log(foo.age)//undefined
```

### Date()对象





### Math对象

```js
Math.PI						// 圆周率
Math.random()				// 生成随机数
Math.floor()/Math.ceil()	 // 向下取整/向上取整
Math.round()				// 取整，四舍五入
Math.abs()					// 绝对值
Math.max()/Math.min()		 // 求最大和最小值
Math.sin()/Math.cos()		 // 正弦/余弦  Math.sin()里面的参数是为弧长！deg*Math.PI/180
Math.power()/Math.sqrt()	 // 求指数次幂/求平方根
```

### 数组方法

##### arr.tostring()

```js
定义：把数组转为字符串

arr.toStirng()
```

##### arr.concact()

```js
定义：链接2个数组

var arr = [1,2,3,4,5]
var arr2 = ["a,b,c"]
console.log(arr.concat(arr2)) // [1, 2, 3, 4, 5, "a,b,c"]
```

##### forEach()

```js
定义：遍历数组，通过回调函数拿数据

var arr = [1,2,3,4,5]
var res =  ''
arr.forEach(function(item,index,arr){
    if(item == 2){res = index}
})
console.log(res)
```

##### arr.every()

```js
    //写一个every的方法
    //every方法是把数组的每一项传到回调函数中去比较函数内部的东西，
    //结果只能返回true or false
    //1.遍历所有项个符合的时候，用 !  ;
    
    var arr = [10, 20, 30, 402];
    Array.prototype.myEvery = function (callback) {
        for (var i = 0; i < this.length; i++) {
            if (!callback(this[i])) {
                return false
            }
        }
        return true
    }
    var res = arr.myEvery(function (item) {
        return item < 50
    })
    console.log(res)

arr.every(funtion(item){ return item>... }) //回调函数里面要加判断！
```



### String对象方法





# js中级

```js
事件
定时器
DOM获取页面元素


```





### BOM

##### location对象

```js
reload()  重新加载当前页面
location.href = 'https://www.baidu.com/'
location.assign = 'https:www.baidu.com/'
location.replace = 'https:www.baidu.com/'
hash 获取url中# 后的字符串，如果没有，则返回空字符串
search
hostname 返回web主机的域名
pathname 返回当前页面的路径和文件名
port 返回web主机的端口
protocol 返回所使用的web协议（http://或https://）
```

##### history对象

```js
back 向后退一页
forward 向前进一页
go --1.参数为 0 或 空 刷新页面；2.参数-1 后退一页 3.参数1 前进一页
length 属性返回历史列表中的网址数
```

##### screen对象

```js
.availHeight 屏幕的高度（不含系统部件高度，就是程序栏）
.availWidth 屏幕的宽度（不含系统部件宽度）
.height 屏幕的高度
.width 屏幕的宽度
```

##### navigator对象

```js
userAgent  返回由客户机发送服务器的 user-agent 头部的值
appName 返回浏览器的名称
appVersion 返回浏览器的平台和版本信息
platform 返回运行浏览器的操作系统平台
```





### 事件

```js
. onfocus 获取焦点事件
. onblur 失去焦点事件
.onmouseover  鼠标移进去
.onmouseout 鼠标移除

```



### 定时器



### 选择器

```js
document.getElementById()
getElementsByTagName()  标签选择器

getElementsByClassName() //类名选择器 
querySelector()
querySelectorAll() 这2个都是根据选择器获取元素，querySelectorAll('.class') // '#id'

+ ~ 选择器
+ 紧邻的下一个兄弟元素
~ 
```

### 卷曲事件

```js
1.获取元素的宽高：
var width  =  element.offsetWidth;
var height = element.offsetHeight;

ele.offsetLeft -- 父级无定位，元素距离左边页面的距离；
ele.offsetTop 

2.卷曲事件：
window.onscroll = function(){
    获取浏览器移动的距离的3种方法：
    document.body.scrollTop //老版本的谷歌浏览器用这个
    document.documentElement.scrollTop // document.documentElement --> html
    window.pageYOffset  ; window.pageXOffset
}

ele.scrollHeight --元素内容的高度(包括卷曲出去的)
ele.scrollWidth --元素内容的宽度


```

### 三、函数

##### 闭包

- 函数内部的函数可以访问其他函数的局部变量，已用特定的局部变量实例的功能叫闭包。

##### 递归

- 函数自调用，类似于循环，但是递归循环的效率较低，比for循环效率低10倍。

```js
exp:
function ff(a){
    fn(b,c){
        if(b==a){
            return c;
        }else if(b>a){
            return null;
        }else{
            return fn(b+5,'(' + c + '+ 5 )')||
                   fn(b*3,'('+ c +'*3)')
        }
    }
    return fn(1,'1')
}

```



