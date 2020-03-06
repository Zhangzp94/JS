### less

怎么用？
    在less文件中写css代码，然后在HTML中引入less.js插件（用来解析less文件）以及引入自己的less文件，引入的时候link中 type='text/less' 。可以加一个自动更新网页样式的监听； <script>less.watch();</script>

自动编译软件：考拉编译

网址：<http://koala-app.com/>

方法：把less文件拖入软件中就会自动编译成css文件了

##### 1、less中的注释

以//开头的注释，编译的时候不会显示

以/* */包裹的注释，编译的时候会显示

##### 2、less中的变量

使用@声明变量，但是less变量有延迟加载特性，less中的变量都是局部变量，引用变量的时候会把局部中变量解析完才引用，下面就是先解析pink再解析red再解析blue，最后再引用变量，结果是blue

````js
@color:pink
{
   .a{
       @color:red
       color:@color //结果是 blue 
       @color:blue
   } 
}
````

@声明的变量是用作样式属性或者选择器的时候要加{ }包裹

````js
@a:color //用作样式属性
.a{
    @{a}:pink
}

@a:.a //用作选择器
@{a}{
    color:pink
}
````

在url( )中也要加 { }

````js
.panel:nth-of-type(@{counter}) {
        @imgsrc: "https://4xiaer.oss-cn-beijing.aliyuncs.com/jy/panel_bg@{counter}.png";
        background-image:url("@{imgsrc}");//这里虽然不在属性、选择器中但是还是加了 { }
    }
````



##### 3、less中的嵌套规则

& 代表的是上一级

````js
.a{
    &:hover{}
}
如果不加& 解析如下：
.a{ }
.a :hover{} 中间多了一空格 错误！
````

##### 4、less中的混合

如下中把重复的代码拎出来，通过调用就是一种混合，就是Mix in

***1)普通混合***

这种混合不加 ( )，会在解析的时候把 .middle 这部分代码也带出来 

````js
//混合
.middle {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  background-color: pink;
  width: 100px;
  height: 100px;
}
.dv1 {
  position: relative;
  width: 300px;
  height: 400px;
  border: 2px solid red;
  margin: 0 auto;
  .inner1 {
    .middle
  }
  .inner2 {
    .middle
  }
}
````

***2)不带输出的混合***

这种就是给.middle加( )  ,那么解析的时候就不会把这部分暴露出来

***3)带参数以及默认值的混合*** 

````js
//混合
.middle(@w:100px,@h:100px) { // 带参数以及默认值
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  background-color: pink;
  width: @w;
  height: @h;
}
.dv1 {
  position: relative;
  width: 300px;
  height: 400px;
  border: 2px solid red;
  margin: 0 auto;
  .inner1 {
    .middle(100px,100px)
  }
  .inner2 {
    .middle(200px,200px)
  }
}
````

***4)带命名参数的混合***

````js
  .inner2 {
    .middle(@h:100px);//因为只传了1个参数，所以用@h: 去写明是传给的第二个参数！
  }
````

***5)less的匹配***

1、假如调用相同名字的混合，如果参数一是 @_ ,那么调用与其同名的时候会自动把这个带上，如下面调用.tringle(L,@w){}的时候会自动带上.tringle(@_){} 

2、下面的案例是画三角形，那么不同方向的三角形自然有多个变量，所以.tringle(L,@w){} 的参数1就是用来匹配的

````js
.tringle(@_){} ,
.tringle(L,@w){} ,
````

````js
--tringle.less文件中

````

##### 5、less继承

less继承是不带变量的

通过 :extend()继承

````js
.public{
  width: 10px;
  height: 10px;
}

.public:hover{
  color: red;
}
---
.a{
  ...
  .b:extend(.public){
    ...
    &:nth-child(1){}
    &:nth-child(2){}
 
}
---
编译后的css

.public,
.a .b,
.a .b:nth-child(1),
.a .b:nth-child(2){
  width: 10px;
  height: 10px;
}
````

````js
.a{
  ...
  .b{
    &:extend(.public) //写在里面
    &:nth-child(1){}
    &:nth-child(2){}
}
---   
.public,
.a .b{
  width: 10px;
  height: 10px;
}
````

````js
.a{
  ...
  .b{
    &:extend(.public all) //加上 all 就会继承所有！
    &:nth-child(1){}
    &:nth-child(2){}
}
    
---
.public,
.a .b{
  width: 10px;
  height: 10px;
}
.public:hover,
.a .b:hover{
  color: red;
}

````

##### 6、less避免编译

直接加  ~" "

````js
{
  width: ~"cacl(100+100px)"  ;
}
--编译后
{
  width:cacl(100+100px) ;
}
````

