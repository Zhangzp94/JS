## ES6

### let 和 const

- let和sonst都是局部变量，块级作用域

- const 用来定义常量，并且不可以修改常量值，但是如果定义的是变量则可以！因为实际是不可以改变指针，定义变量的时候，虽然添加了变量，但是指针不变！

  ```js
  const pi = 3.141592653
  
  const ai = {
      a:10
  }
  pi.b = 20
  ```

### 解构赋值

***1.数组解构赋值***

- 定义：左右一个数组，右边一个数组进行赋值

  ```js
  {}-->块作用域把变量隔离开，避免变量的重复声明！！！
  {
      let a,b;
  	[a,b]=[1,2]
  }
  --
  {
      let a,b，set;
  	[a,b,...set]=[1,2,3,4,5,6]  //加了...输出的是数组
  	console.log(a,b,set);//1,2,[3,4,5,6]  
  }
  --
  变量的交换！es6利用数组解构赋值可以轻易的进行变量的交换，无需像es5那样利用中间变量进行交换！
  {
  	let a=1,b=2;
      [b,a]=[a,b]
  }
  --
  {
      let a,b;
     	function fn(){ return [1,2] }
  	[a,b]=fn()
  	//此场景也可以拿到a,b
  }
  --
  当返回多个变量值的情况下，可以选择自己想要的，这里的`,`代表值 `...`代表数组
  {
      let a,b,c;
     	function fn(){ return [1,2,3,4,5,6] }
  	[a,,,b]=fn();//输出 1 4
      [a,...b] =fn();//输出 1 [2,3,4,5,6]
      [a,,...b]=fn();//输出1 [3,4,5,6]
  }
  ```

***2.对象解构赋值***

```js
{
   let a,b;
   ({a,b} = {a:1,b:2})
   console.log(a,b)
}
--
{
   let {a=1,b=2} = {a:3};
}
--
场景之，后台返回json数据
{
    let data = {
        ab = 'abc',
        test = [{
        title : 'test',
        sole : 'solee'
    }]
    }
	let {ab : a,test: [{title: b}]} = data
    console.log(a,b)
}
--
let {x} = {x:3};// x = {x:3}.x
let {x :b} = {x:3};//赋值的不是x,是里面的b才是变量
let {x=3} = {x:2};//2
```

### 正则

- es5中创建正则对象的方法

  ```js
  var regex = new RegExp(/qwe/i);
  regex.test('qwe123');//true
  
      {
          // regex.flags 方法，获取匹配修饰符
          let a = 'xxx_xx_xx';
          //let regex = new RegExp(/xx/i);es5的写法
          let regex = new RegExp(/xx/ig,'i');//es6的写法
          console.log(regex.flags);//输出  i
      }
      {
          // .exec，匹配到的结果以数组的形式返回
          //g y 都是全局匹配，y是es6的写法，但是执行第二次的时候y是
          let a = 'xxx_xx_xx';
          let a1 = /x+/g;
          let a2 = /x+/y;
          console.log(a1.exec(a),a2.exec(a)); //xxx , xxx
          console.log(a1.exec(a),a2.exec(a)); //xx , null
      }
      {
          // u字符，是在匹配unicode字符的时候用
          console.log(/\uD83B/.test('\uD83B\uDC2A'));
          console.log(/\uD83B/u.test('\uD83B\uDC2A'));//加了u就把后面的看成一个整体了
      }
  ```

### 字符串

- Unicode 字符，每个字符都有对应的二进制代码！

```js
    {
        console.log('\u0061');//a
        console.log('\u{20BB7}');//𠮷
        let s= '𠮷';
        //charCodeAt返回指定位置Unicode编码
        console.log(s.length,s.charAt(1),s.charCodeAt(0));//2 "�" 55362
        let a = '𠮷a';
        //codePointAt 返回的是10进制的码点
        console.log(a.length,s.codePointAt(0).toString(16));

        //字符串中的遍历方法，此方法可以处理Unicode编码大于0FFFF的字符
        let b = '\u{20bb7}abc';
        for (let code of b){
            console.log('es6',code)
            // 𠮷abc
        }
    }
    {
        //判断是否包含的字符的方法，布尔型
        let str = 'string';
        console.log(str.includes('s'),str.startsWith('str'),str.endsWith('ing'));
    }
    {
        //字符串的复制
        let str = 'abc';
        console.log(str.repeat(2));//abcabc
    }
    {
        //变量的拼接 `${item}`
        let q ='qwe';
        let a = 'asd';
        console.log(`i am ${q}${a}`)
    }
    {
        //补白功能，如日期中1 改成01
        console.log('1'.padStart(2,'0'));//2表示输出是2个字符，从前开始补
        console.log('0'.padEnd(2,'2'));//从后向前补
    }
    {
        //标签模板！
        //用处：1.处理多语言转换的时候，中文、英文、德语...
        let user = {
            name:'ak',
            ky:'qwe'
        }
        console.log(abc`i am ${user.name}${user.ky}`)
        function abc(s,v1,v2) {
            console.log(s,v1,v2)
            return s+v1+v2
        }
    }
    {
        //String.raw 阻止换行符换行
        console.log(String.raw`hi\n${1+1}`)
    }
```

### 数值

```js
{
    //判断是否是整数
    console.log(Number.isInteger(20));//布尔类型 true
}
```

### 数组

***Array.from( )***

- 把类数组、家数组转换为真数组

```js
    {
        let arr = {
            '0':'a','1':'b','2':'c','3':'d',length:4
        }
        var q  =[].slice.call(arr);//es5
        var w = Array.from(arr);//es6
        console.log(q)
        console.log(w)

  		 let p =document.querySelectorAll('p');
        var w = Array.from(p);
        w.forEach(function (item,index) {
            console.log(item.textContent,index)
        })     
    }
```

- 映射

  ```js
  Array.from([1,2,3],(item)=>{return item*2});//[2,4,6]
  ```

- fill( )

```js
    {
        //fill方法 ，替换
        //参数只有1个，替换所有；参数2和参数3代表替换的起始和终点位置
       console.log([1,'a','s'].fill(2,1,2))
    }
```

- 遍历数组

```js
    {
        //遍历数组
        //1.2两种存在兼容问题
        for (let index of ['1','2','3'].keys()){
            console.log('获取索引',index)
        }
        for (let values of ['1','2','3'].values()){
            console.log('获取值',values)
        }
        for (let [index,value] of ['1','2','3'].entries()){
            console.log('获取索引和值',index,value)
        }
    }


```

- 查询

```js
    {
        //查询
        //find 只能查找到一个满足条件的东西
        console.log([1,2,3,4].find((item)=>{return item>2}));//3
        console.log([1,2,3,4].findIndex((item)=>{return item>2}))//2 索引
    }
    {
        //查询，布尔类型
        console.log([1,2,NaN].includes(1,NaN));//true
    }
```

### 函数

```js
    {
        //函数参数具有默认值
        let x = 'hihi';
        function fn(x, y = x) {
            console.log(x, y)
        }
        fn();//输出undefined undefined ;必须传参
    }
    {
        //扩展运算符 ...
        console.log(...[1,2,3,'a']);//1 2 3
    }
    {
        function ff(...arg) {
            for (let code of arg){
                console.log(code)
            }
        }
        ff(1,2,3,'a')
    }
```

### 对象

***1.定义对象属性***

```js
    {
        let a = 'ak';
        let b = 'cd';
        let es5 = {
            a:a,
            b:b
        };
        let es6 = {
          a,
          b
        }

    }
    {
        //函数的写法
        let es5_method = function () {
        };
        let es6_method = {
        }
    }
    {
        //新增API
        console.log(Object.is('abc','abc'));// true 判断字符串是否相等
        console.log(Object.is([],[]));//false
        console.log(Object.assign({a:'a'},{b:'b'}));//assign参数2是要被拷贝的对象

        //遍历对象
        let obj = {a:'aa',b:'bb'};
        let arr = [11,22,33];
        for (let [key,value] of Object.entries(obj)){
            console.log(key,value)
        }
    }
```

### set和map数据结构

***1.set***

```js
    //set数据结构，类似于数组，但是不存在重复项
    {
        let arr = [1,2,3,4,2,3];
        let list = new Set(arr);
        console.log(list.size);//4 不包含重复项
    }
    {
        //Set的增加、查询、删除、清空方法
        let arr = ['add','has','delete','clear'];
        let list = new Set(arr);
       // list.add('abc');//添加
       // console.log(list.has('add'));//是否含有此项
       // console.log(list.delete('delete'),list);
        list.clear() ; console.log(list);//清空
    }
    {
        //遍历的一些方法Set也都有|| .keys()  .values() .entries() .forEach()
    }
    {
        //weakSet 只能是放对象，
    }
```



***2.map***

map类似于对象，传统obj是键-值，map是 值 -值也可以！

```js
    {
        let map= new Map()
        let arr = [12,22]
        map.set(arr,123);//设置键是arr,对应的值是123
        console.log(map.get(arr)) ;///123
    }
map.set('t',1)
map.has('t')
map.delete('t')

```



### 类和对象

```js
    {
        class Parent{
            constructor(name,titile){
                this.name = name;
                this.title = titile;
            }
        }
        // class BB extends AA 继承
        //super()子类 传递参数
        class Child extends Parent{
            constructor(name,titile){
                super(name,titile);
                this.type='child'
            }
        }
        console.log(new Child('BB','NN'))
    }
    {
        class Parent{
            constructor(name='ak',titile='ss'){
                this.name = name;
                this.title = titile;
            }
            get longName(){
                return this.name;
            }
            set longName(value){
                this.name =value
            }
        }
        let v = new Parent();
        console.log(v.longName)
        v.longName='andy'
        console.log(v.longName)
    }
    {
        //静态方法,静态方法是通过类调用，不是通过实例调用
        class Parent{
            constructor(name,titile){
                this.name = name;
                this.title = titile;
            }
            static tell(){
                console.log('aaa')
            }

        }
        Parent.tell();//直接通过类调用
    }
    {
        //静态属性
        class Parent {
            constructor(name, titile) {
                this.name = name;
                this.title = titile;
            }

            static tell() {
                console.log('aaa')
            }
        }
        Parent.type = 'test';//直接定义静态属性
    }
```

### Iterator

```js
用于数组遍历
var arr= ['andy','ck']
var map =arr[Symbol.iterator]();//返回一个带有next()方法的对象
cosole.log(map.next());//调用next，如果是false继续遍历，直到true
```

### 模块化

```js
--a.js--
var a = 'aa'
var b = 'bb'
export default {a,b}

--b.js--
import add from './a.js'
console.log(add.a)
```

### Symbol

属于一种数据类型，独一无二的数据类型，`let id  = Symbol()`创建一个id，独一无二的！

### generator函数

```js
function* fn(){
    yield ...;1
    yield ...;2
}
fn.next();1
fn.next();2
```







