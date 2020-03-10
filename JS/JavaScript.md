# 



### 1、节点属性

- 文档：Document

  元素：页面中所有的标签，元素--标签--对象

  节点：页面中所有的内容（标签，属性，文本）-（文本包括 文字、换行、空格）

  根元素：html 标签

- 节点的属性3个：（可以使用标签--元素  点 出来，使用属性 点 出来，使用文本 点 出来）

  1.nodetype ：节点的类型：1--标签 2--属性 3--文本

  2.nodename: 节点的名字：标签节点--大写的标签名字，属性节点--小写的属性名字，文本节点--#text

  3.nodeValue：节点的值：标签节点--null，属性节点--属性值，文本节点--文本内

- 获取节点

  ```js
  //12行代码：都是获取节点和元素的
  var ulobj = document.getElementById("uu");
  //父级节点
  console.log(ulobj.parentNode)
  // 父级元素
  ulobj.parentElement
  //子级节点
  ulobj.childNodes;
  // 子级元素
  ulobj.children
  
  // 第一个节点
  ulobj.firstChild  --ie8中输出的是第一个元素
  // 第一个元素
  ulobj.firstElementChild  --ie8 undefined -以下一致
  // 最后一个节点
  ulobj.lastChild
  // 最后一个元素
  ulobj.lastElementChild
  // 某个元素的前一个兄弟节点
      console.log(my$("three").previousSibling);
  // 某个元素的前一个兄弟元素
  console.log(my$("three").previousElementSibling);
  // 某个元素的后一个兄弟节点
  console.log(my$("three").nextSibling);
  // 某个元素的后一个兄弟元素
  console.log(my$("three").nextElementSibling);
  ```

- 节点的兼容代码

  ```js
  // 获取父级元素的第一个子级元素
  function getfirstElementChild(element) {
      if(element.firstElementChild){
          return element.firstElementChild;
      }else
      // 如果只是ie8，那么可以直接return element.firstChild;
      // 但是有的浏览器不支持上面和这个，所以如下写
          var node =element.firstChild;//获取第一节点
      while (node&&node.nodeType!=1){
          node = element.nextSibling;
      }
      return node;
  }
  ```

  ```js
  // 获取父级元素的最后一个子级元素
  function getLastElementChild(element) {
      if(element.lastElementChild){
          return element.lastElementChild;
      }else
      // 如果只是ie8，那么可以直接return element.lastChild;
      // 但是有的浏览器不支持上面和这个，所以如下写
          var node =element.lastChild;//获取第一节点
      while (node&&node.nodeType!=1){
          node = element.previousSibling;
      }
      return node;
  }
  console.log(getLastElementChild(my$("uu")).innerText)//测试
  ```



### 2、DOM元素的操作

##### 01 插入和删除元素

```js
元素的相关操作方法
var i=0;
my$("bt").onclick=function () {
    i++;
    var obj = document.createElement("input");
    obj.type="button";
    obj.value="按钮"+i;
    // my$("dv").appendChild(obj);
    //把新的子元素插入到上一轮第一个子元素前面
    my$("dv").insertBefore(obj,my$("dv").firstElementChild);
    // my$("dv").replaceChild()//替换
};
my$("bt1").onclick=function () {
    // 移除父级元素中第一个子元素
  my$("dv").removeChild(my$("dv").firstElementChild);

};
my$("bt2").onclick=function () {
    //移除所有父级所有子元素
    while (my$("dv").firstElementChild) {
        my$("dv").removeChild(my$("dv").firstElementChild);
    }
}
```

##### 02 创建元素

***创建元素的三种方式***

1.document.write( )创建元素，缺陷：如果在页面加载完毕，再用这种方式创建元素，那么页面上所有的内容都将全部被清除。

my$("bt").onclick=function(){     document.write("<p>这是个p</p>"); } 

补充：document.write(  )  不仅能创建元素，还可以导入外部网站广告的链接--

2.对象 . innerHTML = " 标签代码及内容"; 

```js
my$("bt").onclick=function(){
    my$("dv").innerHTML="<p>年后的</p>\n";
}
```

3.常用！

```js
 var obj =document.createElement("input");//创建！
 my$("dv").appendChild(obj);//添加！
```

##### 03 获取元素的方式

```js
总结：DOM获取元素的几种方式
根据id属性的值获取元素，返回来的是一个元素对象
-->document.getElementById("id的属性值")
根据标签名字获取元素，返回的是一个伪数组，里面保存的多个DOM对象。
-->document.getElementByTagName("标签的名字")
根据name获取元素，返回的是一个伪数组，里面保存的多个DOM对象。
-->document.getElementByName("name属性值")
根据类样式获取元素，返回的是一个伪数组，里面保存的多个DOM对象。
-->document.getElementByClassName("类样式的名字")
根据选择器获取元素，返回来的是一个元素对象。
-->document.querySelector("选择器的名字")
根据选择器获取元素，返回一个伪数组，里面保存的多个DOM对象。
-->document.querySelectorAll("选择器的名字")
```

##### 04 添加/删除类样式

```js
<div id="dv" class="class1 class2 class3 "></div>
<script>
    let dv = document.querySelector("#dv");
    dv.classList.add("class5")
    dv.classList.remove("class3")
	dv.classList.toggle("class3");//这个是判断入股有class3就删除，没有就新增！
    console.log(dv.className)
</script>
```

##### 05 获取以data-开头的自定义属性

只要是以data-开头的自定义属性都可以使用 ele.dataset 获取属性

````js
<div id="dv" class="class1 class2 class3" data-one="one"></div>

1.获取
console.log(dv.dataset.one);//这里直接写data-后面的即可！如果属性是data-one-tee，那么就要用驼峰命名写：dv.dataset.oneTee

2.设置
dv.dataset.one="two"
````

##### 06 可编辑功能

用户可以在div内打字编辑。只需要给div加 contenteditable="true" 这个属性

```js
<div id="dv" class="class1 class2 class3" data-one="one"
    contenteditable="true"
>1111</div>
```



### 3、事件

##### 01 绑定事件

- addEventListener(type,fn,false) 适用于谷歌 火狐  ，attachEvent  适用于 Ie8，attachEvent  （） 中事件的类型有 On 

- 兼容代码

  ```js
  // 浏览器判断支不支持这个方法，不要带 括号
  // 为任意的元素，绑定任意的事件，--任意的元素，事件类型，事件函数
  function addEventListener(element,type,fn) {
      if(element.addEventListener){
          element.addEventListener(type,fn,false)
      }else if(element.attachEvent){
          element.attachEvent("on"+type,fn)
      }else{
          element["on"+type]= fn;
      }
  }
  ```

- 同一个元素绑定多个不同的事件，指向相同的事件处理函数

  ```js
  my("bt").onclick = f1;
  my("bt").onmouseover = f1;
  my("bt").onmouseout = f1;
  function f1(e) {
      switch (e.type) {
          case "click":
              alert("你好");
              break;
          case "mouseover":
              this.style.backgroundColor="red";
              break;
          case "mouseout":
              this.style.backgroundColor="blue";
              break;
      }
  }
  ```

  

##### 02 解绑事件

***第一种***

```js
my$("bt").onclick=function () {
    console.log('Nihao ');
}
my$("bt").onclick=function () {
    my$("bt").onclick=null;
}
注意：用什么方式绑定事件，就用什么方式解绑事件
1.对象.on事件名字=事件处理函数-->绑定事件；
对象.on事件名字=null;
```

***第二种 -- 谷歌 火狐 支持***

```js
function f1(){
    console.log("nihao ");
};
function f2(){
    console.log("nihao2 ");
};
my$("bt").addEventListener("click",f1,false);
my$("bt").addEventListener("click",f2,false);
//点击第二个按钮，解绑第一个按钮的第一个点击事件
my$("bt2").onclick=function () {
    my$("bt").removeEventListener("click",f1,false)
}
1.绑定事件：对象.addEventListener("没有on的事件名称"，函数名字，false)
解绑事件：对象.removeEventListener("没有on的事件名称"，函数名字，false)

有时候如点击事件，只想产生一次点击事件的时候，就用解绑！
```

***第三种方式 -- ie8支持***

```js
//绑定事件
function f1(){
    console.log("nihao");
}; 
function f2(){
    console.log("nihao2");
};
my$("bt").attachEvent("onclick",f1);
my$("bt").attachEvent("onclick",f2);
//解绑事件
my$("bt2").onclick=function () {
  my$("bt").detachEvent("onclick",f1)  ;
};
// 对象.attachEvent ( "on"+点击事件，命名函数)--绑定事件
//对象.detachEvent ( "on"+点击事件，命名函数)--绑定事件
```

***解绑事件的兼容代码***

```js
//解绑事件的兼容代码
// 为任意的元素，解绑对应的事件
    function removeEventListener(element,type,fnName) {
        if (element.removeEventListener) {
            element.removeEventListener(type,fnName,false);
        }else if(element.detachEvent){
            element.detachEvent("on"+type,fnName);
        }else{
            element["on"+type]=null;
        }
    }
```

##### 03 事件冒泡

- ***冒泡事件定义：***多个元素嵌套，有层次关系，这些元素都注册了相同的事件，如果里面的元素的事件触发了，外面的元素的事件也自动触发了。

  ***阻止事件冒泡：***

  `window.event.cancelBubble=true;`--谷歌 ie8 支持

  `e.stopPropagation`--谷歌 火狐 支持

  ***事件的三个阶段：***

  1.事件捕获阶段：由外向内

  2.事件目标阶段 

  3.事件冒泡阶段：由内向外，如点击三个div，事件效果由里向外依次触发！

  

  ![img](C:/Users/25830/AppData/Local/YNote/data/weixinobU7VjrVn-NQSLDtklCTU8kYHOA0/fdde95d7cdd94f958ba26c3879dbcbc1/9711e155a6414fa0819d60fe67f27e46.jpg)

  ```js
  ele.addEventListener('click',function(e){
     window.event.cancelBubble=true 
  },false)
  ```

  ***案例***

  ```js
  //点击超链接弹出登录框,点击页面的任何位置都可以关闭登录框
   my$("link").onclick=function (e) {
     my$("login").style.display="block";
     //return false;
     //e.preventDefault();
     //上面的两个是阻止默认事件的
     //下面的两个是阻止事件冒泡的
     //window.event.cancelBubble=true;
     e.stopPropagation();
   };
   document.onclick=function () {
     my$("login").style.display="none";
     console.log("隐藏了");
   };
  ```

##### 04 拖拽事件

`onmousedown;onmousemove;onmouseup;`

##### 05 自定义触发事件

- 自定义触发事件，`element.dispatchEvent()`

```js
1.创建
var eve = new Event('myEvent')
element.dispatchEvent(eve)
element.addEventListener('myEvent',function(){})
```

##### 06 事件类型

***1. resize事件***

```js
页面随着窗口大小的变化内部元素而发生大小变化，就是rem
html{
    fontsize:100px
}
window.addEventListener('resize',()=>{
    var scale = document.documentElement.slientWidth/1343  //窗口大小/屏幕大小
    document.documentElement.style.fontsize=100*scale + "px"
    })
```

***2. select事件***

```js
<script>
    /*
    * 当文本框的内容被选中的时候激活该事件！
    * 被选中时，文本框的2个属性，选中的起始和结束位置！
    * input.selectionStart,input.selectionEnd
    * */
    //案例:把选中的内容大写
    var input = document.querySelector("input");
    input.addEventListener('select',selectHandler);
    function selectHandler(e){
        this.value=this.value.slice(0,this.selectionStart)+
            this.value.slice(this.selectionStart,this.selectionEnd).toUpperCase()+
            this.value.slice(this.selectionEnd)
        console.log(input.selectionStart,input.selectionEnd)
    }
</script>
```

***3.change事件***

```js
<script>
    /*
    * 1.change事件，当发生变化的时候触发。
    * selects.selectedIndex;//被选中项的索引值！
    * selects.selectedOptions;//被选中项的内容，是数组
    * multiple //可以多选
    *案例：把选中的项自动填入input中！
    * */
    var selects = document.getElementById('selects');
    selects.addEventListener('change',changeHandler);
    function changeHandler() {
        console.log(selects.selectedIndex)
        console.log(selects.selectedOptions[0].textContent)
        input.value=selects.selectedOptions[0].textContent;
    }
</script>
```

***4.focus/blur事件***

```js
聚焦：focus
失焦：blur
```

***5.input事件***

```js
<script>
    /*
    * input事件，文本框改变的时候触发
    * e.isComposing 是否开启输入法
    * e.inputType 类型！输入、删除
    * e.data 输入内容
    * */
    input.addEventListener('input',inputHandler)
    function inputHandler(e) {
        console.log(e)
    }
</script>
```

***6.`keydown/up`事件***

```js
<script>
    /*
    * e.code 按下的键名
    * e.keyCode 键码值
    * */
    document.addEventListener('keydown',function (e) {
        console.log(e)
    })
    document.addEventListener('keyup',function (e) {
        console.log(e)
    })
</script>
```

***7.scroll事件***

```js
widow.addEventListener('scroll',function (e) {
        console.log(e)
    getScroll().top>100?...:...
})
```

```js
//封装卷曲距离的函数
function getScroll() { 
  return  {
    left : window.pageXOffset||
            document.documentElement.scrollLeft||
            document.body.scrollLeft||0,
    top : window.pageYOffset ||
          document.documentElement.scrollTop||
          document.body.scrollTop||0
  }
}
```





### 4、BOM介绍

##### 01 location对象

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

***案例***

```js
window.onload=function () {
    document.getElementById("bt").onclick=function () {
        // 设置跳转的页面地址
       location.href="http://www.baidu.com"; //属性
        location.assign("http://www.baidu.com");//方法
        location.reload("http://www.baidu.com")//刷新，重新加载
        location.replace("http://www.baidu.com");//直接在当前页面替换跳转，没有后退
        //有历史记录才可以后退，repace是没有历史记录的
    }
}
```

##### 02 history对象

```js
back 向后退一页
forward 向前进一页
go --1.参数为 0 或 空 刷新页面；2.参数-1 后退一页 3.参数1 前进一页
length 属性返回历史列表中的网址数
```

- history 必须要有记录才可以前进 后退

  ```js
      //跳转
      my("bt").onclick=function () {
        window.location.href="03.html";
      };
      //前进
      my("bt2").onclick=function () {
          window.history.forward();
      }
  	//后退
  	my("bt3").onclick=function () {
      	window.history.back();
  	}
  ```

##### 03 screen对象

```js
.availHeight 屏幕的高度（不含系统部件高度，就是程序栏）
.availWidth 屏幕的宽度（不含系统部件宽度）
.height 屏幕的高度
.width 屏幕的宽度
```

##### 04 navigator对象

```js
userAgent  返回由客户机发送服务器的 user-agent 头部的值
appName 返回浏览器的名称
appVersion 返回浏览器的平台和版本信息
platform 返回运行浏览器的操作系统平台

通过platform 可以判断浏览器所在的平台系统
console.log(window.navigator.platform);

判断当前的浏览器是什么浏览器
console.log(window.navigator.userAgent);
```



### 5、正则表达式

- 正则表达式主要是用于***字符串*** 的查找，验证，修改，替换。

- **两种创建方式**：（1）`var reg = new RegExp('a','g');` （2）`var reg = /a/g`

  ```js
  //RegExp(表达式,模式)--2个参数
  // 创建正则表达式对象的方式2种：
  // 1.构造函数创建实例对象
  var per= new RegExp(/\d{5}/);
  var flag = per.test("手机号码：10086")
  console.log(flag);
  //2.字面量创建对象
  var per = /\d{5}/;
  var flag = per.test("手机号码：10086")
  console.log(flag);
  ```

- **正则表达式2种方法：**

  （1）`reg.test(str)` ：true/false

  （2）`reg.exec(str)` ：返回一个数组，返回正则表达式在字符串中符合条件的元素和位置！

- **String字符串方法在正则的运用：**

  ```js
  <script>
      /*
      * String字符串方法在正则的运用
      * str.search(reg);//搜索 返回下标,只返回第一个被找到的下标
      * str.match(reg);//搜索 返回数组，被查到的所有元素的集合
      * str.replace(reg);//替换 返回被替换的字符串
      * str.split(reg);//切割 返回被正则找到切割的数组
      * */
      var str = 'bcdefgahjccas';
      console.log(str.search(/[ac]/g));//查找a和c
      console.log(str.match(/[ac]/g));
      console.log(str.match(/[ac]/g).length);//判断字符被重复的次数
      console.log(str.replace(/a/g, 'A'));
      console.log(str.split(/a/g));//按照a切割，返回切割后的数组
  
      /*
      * []
      * 在 [] 中的字符表示仅选择一个字符，可以理解成或的字符。
      * [ab]表示符合a或b的
      * /[ab][cd]/表示符合 ac ad bc bd 的
      * */
      console.log('[a][c]'.match(/[\[\]]/g))
  
  </script>
  ```

  ```js
  /[^0-9]/g 不包含数字的
  /[\^0-9]/g 包含数字,\ 表示反义
    
  ```

- **去除字符串的空白符**

  ```js
  replace正则匹配方法
  
  　　去除字符串内所有的空格：str = str.replace(/\s*/g,"");
  
  　　去除字符串内两头的空格：str = str.replace(/^\s*|\s*$/g,""); 或者  str.trim()方法
  
  　　去除字符串内左侧的空格：str = str.replace(/^\s*/,"");
  
  　　去除字符串内右侧的空格：str = str.replace(/(\s*$)/g,"");
  ```

- `{} 和?`

  ```js
  "ndkjsnkad".match(/\w{2,4}/g) 表示匹配4个字母的方式匹配  ["ndkj", "snka"]
  "ndkjsnkad".match(/\w{2,4}?/g) 加了?就是非贪婪模式，满足2个就好 ["nd", "kj", "sn", "ka"]
  ```

  

***字符类***

| 元字符 |               释义               |  字符  |              释义               |
| :----: | :------------------------------: | :----: | :-----------------------------: |
|  `.`   |    表示除\n 以外任意一个字符     | [0-9 ] |   查找任何从 0 至 9 的数字。    |
| [a-z]  |             小写字母             | [A-Z]  |            大写字母             |
| [^0-9] |              ^ 反义              | ^[0-9] |           以数字开始            |
|   \b   |             单词边界             | [a-z]$ |           以什么结束            |
|   \d   |    相当于 [0-9] 数字中的一个     |   \D   |             非数字              |
|   \s   |               空白               |   \S   |             非空白              |
|   \w   | 非特殊字符 相当于 `[0-9a-zA-Z_]` |   \W   | 特殊字符 相当于 `[^0-9a-zA-Z_]` |
|   ()   |         分组 提升优先级          |   \|   |              或者               |
|   *    |             0到多次              |   +    |             1到多次             |
|   ？   | 0-1次// 只要出现0或者1次就是true |   ?=   |            紧跟其后             |
|        |                                  |        |                                 |



### 6、cookie

定义：cookie实际上就是一些信息，这些信息以文件的形式存储在客户端计算机上，当用户访问了某个网站，可以通过cookie向访问者电脑上存储数据。为了不在关闭页面的时候，就把数据消失掉，这时候就需要用到cookie。而本地的存储手段有几种，在Application(应用) -- Storage(存储)，里面有本地存储、数据库存储、cookie。

***cookie的存储方式***

```js
document.cookie=''  // '' 中是要存储的数据。

取出cookie ：console.log(document.cookie)

cookie的特点：
1.cookie必须是在服务器开启的时候才能设置和获取。
2.cookie的存储内容不大于5kb
3.cookie是以域名存放区分，不同域不能访问其他域的存储cookie。同域（Ip和端口都相同）下，不同的文件都是可以共享用一个cookie的
4.cookie只能存储数值或者字符串，如果需要存储数组或对象，可以借助 JSON.Stringify() 转化！
5.cookie存储在一个域中的数据是有限的，一般是50条左右。
6.浏览器关闭后（包括后台运行的浏览器）cookie将会丢失，但是我们可以使用过期时间进行存储
方法：
var date = new Date()
date.setDate(22) //比如设置cookie有效期到22号
document.cookie = JSON.Stringify(obj)+';expires='+date.toString() 
7.cookie的存储不能跨浏览器，不同的浏览器要设置不同的cookie。
```

***自动登录的原理***：

​		第一次打开页面的时候，先查看该域下cookie中有没有token（服务器查看）。如果没有，输入用户名、密码登录，服务器根据数据库验证成功后，会主动存储token（就是一个令牌）到该域的cookie中，那么该域下的cookie中就有token啦，下次就会自动登录了！

***为什么要使用cookie？***

​		因为cookie可以自动发送数据，当打开一个网站的时候，如果当前cookie中存储该域下的内容，cookie会自动将当前域下的cookie内容直接发送到页面中去，同样服务器页面也可以设置cookie！

***cookie的缺陷***

​		浏览器可以设置屏蔽和清除cookie!

***cookie当今使用的范围？***

​		比如页面中的广告，一般使用`<iframe src=""></iframe>`标签！这样引入比如JD 的链接，那么就会拿到jd的cookie，而cookie中自带有你经常搜索的一些关键字信息，就会在广告区域弹出你的喜好！



### 7、XSS攻击

定义：在表单评论的时候，如果用户输入的内容为 `<script>alert(document.cookie)</script>` 或者

`<a href='javeScript:;'><img src="..." /></a>`   的时候，其他用户一旦点击，就会把自己的cookie内容发送给攻击者！

解决方法：前端处理可以在submit提交的时候，对Input的value值是否有script或者javascript: 这样的字符串（转换成大小写后判断），如果有就需要替换这个文本内容，比如把script替换成 [remove]等。 

### 8、webStorage

webStorage分为2种：localStorage (本地长期存储) 和 sessionStorage (本地即时存储)

***1.localStorage***

```js
设置：
localStorage.user=10
localStorage.setItem('user',10)
获取：
localStorage.user
localStorage.getItem('user')
清除：
localStorage.removeItem('user');//清除某个key
localStorage.clear();//清除所有
```

localStorage特点：
		localStorage存储的是字符串，存储限制是5M，并且不会根据http请求发送给服务器(cookie会)，根据域存储，不同的文件可以访问到同域下的localStorage，但是不可以访问到sessionStorage！不同浏览器获取不到相同的localStorage 和 sessionStorage！









### N:知识点链接

##### （1）JS继承的6种方式

https://www.cnblogs.com/Grace-zyy/p/8206002.html 

1.克隆节点

```js
1.找到要克隆的元素 ele
ele.cloneNode('false');
ele.cloneNode('true'); //true 是克隆节点的所有子级节点

ulobj.children[0].cloneNode(true)--克隆olobj下的第一个节点
ulobj.appendChild(ulobj.children[0].cloneNode(true));--克隆的节点添加到ulobj
```

2.焦点事件

```js
1.获取焦点和失去焦点
//案例模拟搜索框
<input type="text" value="请输入搜索的内容" id="ak">
<script>
    //获取焦点框事件
    document.getElementById("ak").onfocus=function () {
        if(this.value=="请输入搜索的内容"){
            this.value="";
            this.style.color="black";
        }
    }
    //失去焦点框事件
    document.getElementById("ak").onblur=function () {
        // 下面写length有利于浏览器的优化
        if(this.value.length==0){
            this.value="请输入搜索的内容";
            this.style.color="gray";
        }
    }
</script>
```

3.设置、获取、删除元素属性

```js
//总结:设置自定义属性:setAttribute("属性的名字","属性的值");
//获取自定义属性的值:getAttribute("属性的名字")、
//移除自定义属性:removeAttribute("属性的名字")
```

4.设置文本内容 `setInnerText`

```js
设置文本内容：
var pobj = document.createElement("p");
setInnerText(pobj,temp[i])
```

5.`Object.assign()`方法

```js
可以设置属性！
Object.assign(li.style,{
    margin:0;
    float:'left';
  	fontsize:0;//去除图片的间隙 
})
```







