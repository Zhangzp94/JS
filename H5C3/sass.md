### sass

ruby的安装：sass是基于ruby语言的，先要安装ruby，

https://pan.baidu.com/s/1ADVhh0FtmJ9l_z-gm2udug  密码：kx06

sass安装：npm i sass -g

<https://www.runoob.com/sass/sass-install.html>

1.webStorm可以实现自动编译功能

设置--工具--file Watchers

````
Arguments:   
$FileName$:../css/$FileNameWithoutExtension$.css

Output paths to refresh:  $FileNameWithoutExtension$.css:$FileNameWithoutExtension$.css.map
````

2.cmd自动编译功能

````js
创建scss文件夹以及css文件夹
sass
	scss
    	a.scss
	css
    
    
cmd中
cd到 sass
输入 sass --watch scss:css 即可！

````

##### 1、sass的输出css格式

````js
sass --watch scss:css --style compact //单独样式在同一行，紧凑型
sass --watch scss:css --style compressed  //输出全部在一行显示，压缩型
sass --watch scss:css --style expanded   //输出的就是我们平时写格式
````

##### 2、变量

以  $  符号声明变量！

属性中使用  #{ $变量 }

##### 3、父选择器

用 & 符号表示！

##### 4、属性的嵌套

加 { }

````js
border: 1px solid #ccc{
  left: 2px;
  right: 2px;
};

编译后
  border: 1px solid #ccc;
  border-left: 2px;
  border-right: 2px;
````

##### 5、mixin-混合

````js
@mixin a($变量){} @mixin定义
{
	@include a(){} //@include调用    
}
````

##### 6、@extend 继承

````js
.public{
  width: 100px;
}
.public a{
  height: 200px;
}
.a{
  @extend .public;//
  height: 20px;
}

--
.public, .a {
  width: 100px;
}

.public a, .a a {
  height: 200px;
}

.a {
  height: 20px;
}
````

##### 7、计算 和字符串函数

sass 8/2 编译后还是 8/2   ，（8/2）编译后是4

````js
字符串函数：
$string:"hello nihao";
b{
  content: to-upper-case($string);
  content: to-lower-case($string);
  content: str-length($string);
  content: str-index($string,'hello');
  content: str-insert($string,'.net',12);
}

编译后：
b {
  content: "HELLO NIHAO";
  content: "hello nihao";
  content: 11;
  content: 1;
  content: "hello nihao.net";
}
````

##### 8、@if...@else

````js
@abc:red;
@if @abc ==red{
    ...
}@else if @abc == black {
    ...
}@else{
  ...  
}
````

##### 9、@for ... from ... through

@for $var from <开始值> through <结束值> {  }

@for $var from <开始值> to <结束值> {  } //to 表示

````js
$a:4;
@for $i from 1 through $a {
    .col-#{$i}{}
}
````

##### 10、@each...in...

对列表数据的遍历输出

````js
@cions:a b c ;
@each @icon in @icons {
    .col-#{@icon}{}
}
````

##### 11、@while

````js
$i:6;
@while $i > 0 {
    .item#{$i}{  };
    $i:$i-2;
}
````

##### 12、函数

````js
$colors:{light:#fff,dark:#000};
@function color($key){
    @return map-get($colors,$key)
}

 body{
     color:color(light)
 }        
````

