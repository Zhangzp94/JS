# html

### html标签

```
1. 引用标签：<q>你好</q> // "你好"
2. 地址标签：<address>联系地址信息</address>
3. 加粗标签：<b> </b>  或 <strong> </strong>
4. 斜体标签：<i> <i>  Italic [ɪˈtælɪk] 或 <em> </em> emphasis [ˈemfəsɪs]  
5. 删除标签：<s> <s> 或 <del> <del>
6. 下划线标签：<u> <u> 或 <ins> <ins> 
//3-6的标签 后面的语气更重
7. 空格键：&nbsp; 
8. <code>加入的代码语言</code>
9. <pre>加入大段代码</pre>
10. 表格标签：<caption>个人信息</caption> 放在<table>标签中，会顶部居中
11. 文本域标签：<textarea disabled style="resize: none;"> css样式：resize是让文本域禁止拖动，	 html属性：disabled 是只读模式。
12.


//单标签
1. 换行标签：<br /> 
2. 分割线标签：<hr /> exp:<p>今天早上 </hr> 吃饭了嘛</p> //会有一条分割线
3. 
```

##### \<a>\</a> 标签

```html
1.href后面可以加网址，要带http，target属性表示跳转方式，默认当前页面打开，'_blank'表示在新的窗口打开。
<a href="http://www.baidu.com" target="_blank">百度</a>

2.href中加 '#' 表示空链接，不会跳转。
<a href="#" target="">百度</a>

3.锚定 //重点
  href='#id'，在需要跳转到的地方标签设定Id即可实现锚定跳转。
<a href="#one">点击这里</a>
<p id='one'>跳转到这里</p>
```

##### \<table>\</table> 标签

```html
1.属性 align='' 表格对齐方式， cellspacing=''单元格和单元格之间的距离， cellpadding=''文字内容距离单元格的距离
2.<th></th>表头，自动加粗 ， <thead>头部</thead>， <tbody>身体</tbody>

<table width="100" height="200" border="1" align="center" cellspacing="0" cellpadding="0">
    <thead>
        <tr>
            <th>表头</th>
            <th>表头</th>
            <th>表头</th>
         </tr>
    </thead>
    <tbody>
        <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>
```

##### \<input> 单标签

![img](file:///C:\Users\25830\AppData\Local\Temp\ksohtml18624\wps3.jpg)

```html
1.input标签只读属性
<input type="text" value="只读"  disabled>
<input type="text" value="只读"  readonly>

2.和label标签配合使用，点击label标签即可跳转到input标签
<label for="username">用户名</label>
<input type="radio" id="username">

3.
```



##### \<td>\</td>标签

```html
1.属性colspan=''按列，左右合并，合并多少列，就去掉多少列   
<tr>
        <td colspan="2">你好</td>
        <td>你好</td>
        <!--<td>你好</td>-->
</tr>

2.属性 rowspan=''按行，上下合并，合并多少行，就去掉多少行
    <tr>
        <td rowspan="2">你好</td>
        <td>你好</td>
        <td>你好</td>
    </tr>
    <tr>
        <!--td rowspan="">你好</td>-->
        <td>你好</td>
        <td>你好</td>
    </tr>
```

##### \<img /> 单标签

```html
1.主要是 alt 和 title 属性
<img src="路径"/> 带有路径的图像；
<img src=”错误的路径” alt="文字描述"/>；
<img src=“路径”  title="文字描述" />;鼠标移到图像显示文字描述
```

##### \<ol>\</ol>有序标签

```html
1.ol有序列表标签带1.2.3...序号的，和ul标签一样，ol后面紧跟li标签，li里面可以跟其他标签
<ol>
    <li>你好</li>
    <li>你好</li>
    <li>你好</li>
</ol
```

##### \<dl>\</dl>自定义标签

```html
<dl>
    <dt>上海</dt> //注意这里是 <dt>
    <dd>宝山</dd>
    <dd>杨浦</dd>
    <dd>长宁</dd>
    <dd>浦东</dd>
    <dd>嘉定</dd>
</dl>
```

##### \<select>\</select>选项标签

```html
<select name="" id="">
    <option value="">你好</option>
    <option value="">你好</option>
    <option value="">你好</option>
</select>
```

##### h5新标签

![img](C:/Users/25830/AppData/Local/YNote/data/weixinobU7VjrVn-NQSLDtklCTU8kYHOA0/ba0ca76054ec4454b64f063fbbb4cd2b/d62497a84b1b44a59cd448d436877b91.jpg)

![img](C:/Users/25830/AppData/Local/YNote/data/weixinobU7VjrVn-NQSLDtklCTU8kYHOA0/f71f2b2957a844c68500e5a7e7dc83a1/f7dadd7ee28d409ca5feb5296149612c.jpg)



### html特殊字符集

![img](file:///C:\Users\25830\AppData\Local\Temp\ksohtml18624\wps2.jpg)

### 选择器

```html
1.类选择器
<div class="class"></div> css--> .class{}
2.id选择器
<div id="id"></div> css--> #id{}
3.后代选择器
div p {}
<div>
    <p></p>
</div>
4.子代选择器
div > p{}
<div>
    <p></p>
</div>
5.交集选择器
div .p {}
<div>
    <p class='p'></p>
</div>
6.并集选择器
div,p{}
<div>
    <p class='p'></p>
</div>
7.+号 选择器
li + li{
    选择的是下面第一个li后面所有的li元素,但如果有其他标签，就终止
}
<li></li>
<li></li>
<li></li>
<a></a>
<li></li>
8. ~号 选择器
用法类似 +号，不同的是如果有其他标签，会跳过这个a标签，继续向后选择一样的标签。
```

##### 属性选择器

```js
img[src]{};//包含src属性的元素
div[class*="text"];//所有，记得加" "
div[name="username"];

<input type="checkbox" name="items"/> //可以这么选择 :checkbox[name=items]
```



![1571831512849](C:\Users\25830\AppData\Roaming\Typora\typora-user-images\1571831512849.png)

##### 结构伪类选择器

```html
div:last-child 最后一个元素
div:first-child 第一个元素
div:nth-child(even) 偶数 （odd) 奇数
div:nth-child(3) 第三个
div:nth-child(Xn +1) 表达式写法

:first-of-type \ div p:nth-of-type()  //div里面 从p元素开始数，就是从type为p的元素开始
```

![img](C:/Users/25830/AppData/Local/YNote/data/weixinobU7VjrVn-NQSLDtklCTU8kYHOA0/80b55143faee46698784cb9009b8f16e/36aae615a09c43348eeb30d10bb850a4.jpg)





# CSS

### 样式

##### css字体样式

```html
    <style>
        .class{
            width: 100px;
            height: 100px;
            border: 1px solid red;
            font-size: 20px;
            font-family: "微软雅黑 Light";
            font-weight: 700; //字体加粗
            font-style: italic; //字体倾斜

        }
    </style>
```

##### css综合样式

![img](file:///C:\Users\25830\AppData\Local\Temp\ksohtml18624\wps4.jpg)

![img](file:///C:\Users\25830\AppData\Local\Temp\ksohtml18624\wps5.jpg)

##### display 样式

```html
1.text-align:center; 运用水平居中样式的时候，必须行内元素或行内块元素
{
display:inline //转为行内元素
display:block //转为块级元素 ，或 有显示的意思
display:inline-block //行内块元素
}
{
display:none //隐藏
display:block //显示
}
```

##### 伪链接

```html
        a:hover { //鼠标经过
            color: red;
        }
        a:visited { //鼠标点击后
            color: green;
        }
        a:active { 鼠标按下时候的样子
            color: black;
        }
        a:link { //初始未点击
            color: blue;
            text-decoration: none; //取消下划线
        }
```

##### 背景样式

```html
    div{
        background-image: url('./img.png');
        background-repeat: no-repeat; //不平铺， repeat-x ,repeat-y
		background-position: top right;/位置写2个，可以写实际距离 10px 20px
        background-attachment: fixed;//背景固定，当背景图片较大的时候，默认是滚动的 scroll
    }
	div{ //连写
		background: 背景颜色 URL 平铺方式 背景滚动 背景位置;
		background: #000 url('') repeat-x fixed 10px 20px;
		background: rgba(0,0,0,0.3) //透明度
	}

补充1：插入背景图片的时候，样式记得加 { width:100%; } ,不然不显示大小！

补充2：背景图片按内容裁剪（一般用于点击小图标的时候，点击周围也可以显示点击中）

a链接设置 40 *40
设置padding值 -- >这一步是让背景图片从中间内容开始平铺
background-origin: content-box /*让背景从内容开始平铺*/
background-clip: content-box; //裁剪！！!
/*默认的就是
border-box  边框以外被裁剪掉
padding-box 内边距以外被裁剪掉
content-box 内容以外被裁剪掉
*/
```

##### 边框样式 border

```html
1.属性：solid 实线；dotted 点线；dashed 虚线；
div{
	border:1px solid red;
	//分开写
	border-color: red;
	border-width: 2px;
	border-style: solid;	
	//位置
	border-top: ;
	border-bottom: ;
	border-right: ;
	border-left: ;
	//圆角
	border-radius:50%; //表示边框角的圆角
	border-radius:2px 2px 2px 2px; //左上 右 下 左下 顺时针
	border-radius:2px 2px; //左上和右下  右上和左下
}

2.table表格中
{
	border-collapse:collapse; //表示table表格的边框没了
}
<table width="100" height="200" border="10"></table>
```

##### 盒子阴影 box-shadow

```html
X轴（必须有）、 Y轴（必须有）、 阴影虚实 、阴影大小
box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.1);//透明度，这里的透明度表示的是阴影
box-shadow: 0px 15px 30px  rgba(0, 0, 0, 0.1);//常用的例子；这里省略了阴影大小

a:hover{ 
box-shadow: 0px 15px 30px  rgba(0, 0, 0, 0.1);
transition: all 1s; 
}  //鼠标经过，阴影慢慢显现
```

##### 盒子内减模式

```html
定义：有时候给了padding 值会撑开盒子，给盒子加box-sizing:border-box 就不会撑开盒子
{
	box-sizing:border-box;
}
```

##### 鼠标样式

```html
cursor: pointer; // 让鼠标样式变成小手
cursor: text; // 让鼠标移动文本字体的时候，可以选择文字
cursor: move; //鼠标变成移动的十字架
```

##### 文本框去边框

```html
input标签中如何去掉蓝色边框！
{
outline: none
}
```

##### 垂直对齐方式

```html
1.vertical-align: ; 样式主要是针对行内元素和行内块元素
2.图片默认的和盒子底线有个3像素的空隙，vertical-align: top; 或者 middle; 两种垂直对齐方式解决！
	或者把img转为块元素，块元素默认无间隙
```

![img](C:/Users/25830/AppData/Local/YNote/data/weixinobU7VjrVn-NQSLDtklCTU8kYHOA0/e2eb482fda3544e28df0504c47055431/47c4693212654b26a26c52e155efb9c3.jpg)



##### 元素的显示和隐藏

```html
1.显示和隐藏3种方式：
	display:none / block 
	visibility:hidden / visible
	overflow:hidden 隐藏/ bisible 显示/ scroll 加滚动条/ auto 内容超出父级边框会显示滚动条 
```

##### 文字的溢出和隐藏

```html
1.做出下图的效果
{
white-space:nowrap;//强制在同一行内显示所有文本，直到文本结束或者遭遇br标签对象才换行   
overflow:hidden; //溢出的部分隐藏
text-overflow: ellipsis;  //溢出的文本用省略号代替
  /以上三句代码缺一不可/
}
```

![img](C:/Users/25830/AppData/Local/YNote/data/weixinobU7VjrVn-NQSLDtklCTU8kYHOA0/14571d612f364336b8ebf9c4e642c71c/30ff2286d10845c68a72f24aa0bee8d7.jpg)





### 布局

##### 浮动

```html
1.定义：浮动是把行内元素和块元素转为 行内块元素。
exp:
上下3个盒子，1、2两个盒子浮动，那么第三个盒子自动顶到第一个盒子的位置，如果第三个盒子内有图片或文字那么内容不会上浮，会紧挨着1、2两个盒子。
2.清除浮动
为什么要清除浮动：为了解决子级元素浮动（如ul li，设置li浮动），父级元素的高度就自动为0的情况。
清除浮动后，父级元素的高度 等于 子级元素的高度。
3.清除浮动的方式：
	a.在最后一个标签加clear，<div style="clear: both"></div>
	b.在父级元素样式加：overflow:hidden
	c: 把样式加在需要清除浮动的父级元素中
	.clearfix:after{
        content: '';
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
    }
    .clearfix{
        *zoom: 1; //加了* ,ie7以下的版本就会识别
    }
	
```

##### 伪元素

```html
定义： :after :before 这样的，可以在元素中加东西，必须要带content属性
exp:
//给p1加一个图片，
//给p2设置三角形，
css--
    .p1:before{
        content: '';
        background: url('./img.png') no-repeat;
        width: 20px;
        height: 20px;
        display: inline-block;
    }
    .p2{
        border:30px solid transparent ; //transparent： 透明的
        border-left: 30px solid red;
    }
html--
<p class="p1" style="color: red">html</p>
<p class="p2"></p>
```

案例：

![1571832910523](C:\Users\25830\AppData\Roaming\Typora\typora-user-images\1571832910523.png)

##### 合并外边距

```html
1.子级、父级盒子中父级盒子没有边框的时候，子级盒子设置margin-top样式时，会把父级盒子带下来，这种情况叫合并外边距。
解决方案：1)给父级盒子加border; 2）给父级盒子加overflows:hidden; 3)给父级盒子加内边距：padding-top:1px

```

##### 定位

```html
1.默认状态是静态定位，position:staic,如果元素是绝对、相对、固定定位，可以通过这个取消。

2.相对定位：position:relative 
3.绝对定位：positon:absolute
4.固定定位：position:fixed
绝对定位和固定定位不占位置，类似于浮动，玩定位的时候一般父级元素设置相对定位，子级元素绝对定位。子绝父相！
补充：绝对和固定定位因为不占位置，所以宽度和内容有关，不显示宽度的时候，加 width:100%

5.层级 z-index
定义：用来定位元素的，z-index=...,值越大元素越在上，只有绝对定位和固定定位有此属性

```

##### 置换元素

```html
1.行内元素的宽高和内容有关，但是Img input这样的行内元素例外
img input textarea select 这样的元素为置换元素，定义：根据元素属性来显示的元素，如Img通过src...
```

##### 行内元素

```html
行内元素特点：
1.设置宽高无效，水平方向的padding值和margin值可以设置，但是垂直方向无效。
2.行内元素只能容纳其他行内元素或文本内容，但是 <a></a>元素比较特殊，可以容纳块级元素。
```

##### 搜索引擎优化

```html
1.三大标签的优化：name中是对整体项目的关键字描述
<title> </title>
<meta name="description" content="....." />
<meta name="keywords" content="...." />

2.网页logo的优化： 一般我们在设置logo的时候用 <h1>标签设置，
    exp:
<div class="logo">
    <h1>
        <img src=" " title= " " alt=" " >
    </h1>
</div>
```



### 字体图标的使用

##### 第一种方法

1.打开字体图标网站：<https://icomoon.io/app/#/select>

2.选择使用的图标下载，把里面的fonts文件夹复制到项目文件中

![1571829201714](C:\Users\25830\AppData\Roaming\Typora\typora-user-images\1571829201714.png)

3.打开 demo.html ，复制里面内容

![1571829393796](C:\Users\25830\AppData\Roaming\Typora\typora-user-images\1571829393796.png)

4.给样式初始化，加如下代码,在icomoon文件中的style.css中有

```html
@font-face {
  font-family: 'icomoon';
  src:  url('fonts/icomoon.eot?6o5pn0');
  src:  url('fonts/icomoon.eot?6o5pn0#iefix') format('embedded-opentype'),
    url('fonts/icomoon.ttf?6o5pn0') format('truetype'),
    url('fonts/icomoon.woff?6o5pn0') format('woff'),
    url('fonts/icomoon.svg?6o5pn0#icomoon') format('svg');
  font-weight: normal;
  font-style: normal;
  font-display: block;
}
```

5.被引用的元素要加font-family

![1571829640871](C:\Users\25830\AppData\Roaming\Typora\typora-user-images\1571829640871.png)

##### 第二种方法

1.![1571831055687](C:\Users\25830\AppData\Roaming\Typora\typora-user-images\1571831055687.png)



##### 使用自己的字体图标

1.点击下图按钮，上传我们的  svg格式的图片，然后就正常下载使用即可

![1571829903078](C:\Users\25830\AppData\Roaming\Typora\typora-user-images\1571829903078.png)



2.如何追加字体图标：--当我们要使用新的字体图标的时候，但是又不想删除原来的字体图标包，此时可以打开图标网站，同样是点击上图中的 Import Icons 按钮，上传 icomoon文件中的.json文件即可！

### CSS高级技巧

##### 精灵技术

```html
1.精灵技术就是引入一张大背景图，通过移动背景图片的坐标
优点：减少对服务器的请求次数
缺点：修改一个图，就全部修改
```

##### 滑动门案例

```html
1.行内块元素的宽度和内容有关！行高会继承！给了行高可以不用给高！
2.为什么用滑动门：因为里面的字数不一样多，不方便设置宽度；

```

![img](C:/Users/25830/AppData/Local/YNote/data/weixinobU7VjrVn-NQSLDtklCTU8kYHOA0/bc9b4833f390494a880c6c560f6773e6/fd3e6d8bbc1d43e580cda15cbe063835.jpg)

