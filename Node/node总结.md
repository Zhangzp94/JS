node总结

说明：传统js文件需要依赖html运行，但是Node可以不依赖html，node是使用C++开发的，借助了V8引擎，在服务器用了V8引擎帮助解析js代码直接在电脑上执行Js代码了，不需要像传统上需要依赖浏览器！所以可以 node xx.js 指令运行js文件！



js代码怎么执行？js代码先生成一个字节码--->浏览器运行，谷歌的V8引擎是直接把js代码翻译成二进制代码运行，所以谷歌打开浏览器快很多！

### 安装：

Node下载官网：<https://nodejs.org/en/download/>

Node文档：<https://nodejs.org/api/buffer.html>

安装node软件，cmd 输入 node --version 看是否安装成功，node xxx.js 直接运行node文件

注意：node需要配置环境变量（配置后就可以在任何的目录下启动node）

高级设置-环境变量-path-编辑-把nodejs.exe的目录复制进去

### API集合

##### 读取文件：

```js
fs.readFile('./xxx.js','utf8',function(err,data){
    if(err){
        return ...
    }
    ...
})
//注：读取文件返回的data是二进制代码，需要转成字符串，要么data.toString(),要么加 'utf8'
```

##### 读取文件目录：

```
fs.readDir('xxx/xxx',function(err,file){
	
})
```

##### 写入文件：

```js
fs.writeFile(URL,数据文件,'utf8',function(err){
    if(err){
        return ...
    }
    ...
})
//参数1：写入文件的路径，参数2是写入的数据，
```

##### fs.mkdir()

```js
创建文件目录：
fs.mkdir('./目录',function(err){
    if(err){
        return err
    }
    fs.mkdir('./目录/01');
    fs.mkdir('./目录/02');
    fs.mkdir('./目录/03');
})
```

##### 删除文件 fs.unlink()

```js
fs.unlink('./xxx.js',function(err,data){
    if(err){
        return ...
    }
    ...
})
```



##### json方法

JSON.stringify( )  ：对象转为字符串

```js
//一般可以用来把对象转为字符串，再保存到文件中，如下：
var data = JSON.stringify({studets:students})
fs.writeFile(URL,data,function(err){
    if(err){
        return ...
    }
    ...
})
```

JSON.parse( ) : 把json文件转化成对象

```
//一般是读取json文件后，输出是字符串形式，所以需要转成对象去操作数据
fs.readFile('./xxx.js','utf8',function(err,data){
    if(err){
        return ...
    }
    var data = JSON.parse(data).xxx //这是对象 可以点出里面的属性
})
```

```
JSON的格式
{
	data:[
		{},
		{},
		{}
	],
	data2:[
		{},
		{},
		{}
	]
}

```

##### forEach()方法

- 用于数组的遍历，结果返回给回调函数，参数1为数值，参数2为索引

  ```
  arr.forEach(function(item,index){
  	console.log(item,index)
  })
  ```

##### arr.find() , arr.findIndex()

```
// arr = [{},{},{}]
//当你传入进去的id值 === 遍历的这个数组中的对象item的id时候，返回这个item对象
var obj = arr.find(function(item){
	return item.id  === id
})

//当你传入进去的id值 === 遍历的这个数组中的对象item的id时候，返回这个item对象的索引值
var index = arr.findIndex(function(item){
	return item.id  === id
})
```

##### es6中插入变量

```
es6中， ` ${ item } ` 去代替原来用 双引号去拼接的方法
```

##### __dirname 和 __filename

```js
exp:运行node.js文件！
__dirname :当前正在执行的js的文件目录 c:/veiws
__filename:当前正在执行的js的文件完整目录 c:/veiws/node.js

一般用path模块一起用
```



### 原始Http写法

原始node不引入express的 http的写法；

```js
var http = require('http')
var server = http.createServer()
server.on('request',function(req,res){
   // req.url 是客户端请求的url地址
   if(req.url=='/'){
       //设置响应头是因为系统默认是中文自带解析，需要告诉系统响应的是什么类型，防止解析乱码
       res.setHeader('Content-Type','text/plain; cherset=utf-8')
       //res.write('success')
       res.end('你好')
   } else if(){  
    res.setHeader('Content-Type','text/html; cherset=utf-8') 
  	res.end('<p> 点击 </p>') //要以 end() 结束，不然页面会一直转圈,res.end('success')     
   } else if(){
    res.setHeader('Content-Type','image/jpeg') 
  	res.end(data)       
   }
})
server.listen(3000,function(){
    console.log('running...go')
})
```



##### url模块

```js
适合get提交方式！
var url  = require('url') // 这个核心模块不需要下载
定义：是用来创建对象，获取get方法提交上来的数据，
var obj = url.parse(req.url) // 获取的是form表单 /...?vaule=ck$age=12
其中 obj.url = ?前面路径
	obj.query = ?后面的内容

/add?value='12'&name='ck'
补充：obj.query 也是一个对象，里面是 {value:12 , name:'ck'}
```

##### 服务器端解决响应报文乱码问题

```js
服务器端是utf8编码的，但是客户端处理响应报文的时候是浏览器默认的编码格式，所以有时候会出现乱码的形式！

解决方法：通过服务器设置响应报文头，告诉浏览器用utf8编码解析！
参数1：数据类型，数据类型内容，详情可参考上面原始方法的写法！
res.setHeader('','')

补充：
设置res.setHeader('Content-Type','mime.getType('响应的文件名')') 的时候，可以用mime模块。直接npm搜mime！

fs.readFile(filename,function(err,data){
    if(err){
        res.end('404')
    }else{
        res.setHeader('Content-Type',mime.getType(filename));
        res.end(data)
    }
})
```

##### request对象常用属性

```js

request.headers //返回对象，对象中包含所有请求报文头
request.rawHeaders //返回数组，数组中包含请求报文头字符串
request.httpVersion //获取请求客户端所使用的http版本
request.method //获取客户端请求的方法（POST GET）
request.url //获取客户端请求的路径

其他可以在node官网查找 http->http.IncomingMessage 类
```

##### response对象常用属性

```js
res.end();//()内只能是String 和Buffer类型
res.write();
res.setHeader('Content-Type','text/plain;charset=utf-8');//设置响应头
res.statusCode = 404;//设置响应的状态码
res.statusMessage = 'Not Found';//	设置状态信息

res.writeHead(404,'Not Found',{'text/plain;charset=utf-8'})
其他可以在node官网查找 http->http.ServerResponse 类


补充： res.setHeader('Location','/');//http原生写法的重定向
	express中用  res.redirect('/');//异步请求中不可用！
```

##### mime模块

- mime模块可以一键设置响应头的格式

  ```js
  1.安装mime模块
  2.引入mime模块， var mime = reuqire('mime');
  
  res.setHeader('Content-Type',mime.getType(req.url));
  ```

##### 第一次读取文件数据

```js
因为第一次读取文件的时候没有数据，要如下写
fs.readFile(path.join(__dirname,'data.json'),'utf8',function(err,data){
    //第一次读取文件的时候data.json不存在肯定会报错，为了不结束进程，加错误码不是 ENONET;
    if(err && err.code!=='ENONET' ){ throw err };
    //第一次读取json文件的时候没有文件，不能直接转为数组，所以加 []
    var list = JSON.parse(data || '[]');
    
    ...
})
```

##### post提交数据

```js
var queryString = require('queryString')

fs.readFile(path.join(__dirname,'data.json'),'utf8',function(err,data){
    if(err && err.code!=='ENONET' ){ throw err }; 
    var list = JSON.parse(data || '[]');
//因为post提交数据数据量很大，所以是分批提交分批接收，这时候需要监听request的data事件和end事件。
//
    var arr = [];
    req.on('data',function(chunk){
        //chunk是每次提交的数据，类型是Buffer对象,每次的数据都放到数组中
        arr.push(chunk);
    })
    //当end事件触发的时候，arr中所有的数据汇总就好了
    
    req.on('end',function(){
    //先把数组arr中的数据汇总
   		var postBody = Buffer.concat(arr);
        postBody = postBody.toString('utf8');
        //因为post提交这里是通过请求头提交的都是 & 的形式，把 post 请求的查询字符串，转换为一个 json 对象,需要引入 querystring 模块。
      	postBody = queryString.parse(postBody);
        list.push(postBody)
    })
    ...
})
```

##### Buffer对象

- 分批传输数据，通过Buffer传输一部分数据。Buffer里面存放了字节！

- 创建1个Buffer对象 ，通过 Buffer.from(  )

  ```js
  通过一个字节数组来创建 Buffer对象
  var arr = [0x68, 0x65, 0x6c];
  var buf = buffer.from(arr);
  buf.toString('utf8');
  ```

- 拼接一个BUffer对象

  ```js
  var buflist = [buf1,buf2]
  Buffer.concat(buflist);
  ```

- 获取字符串对应的字节个数

  ```js
  var len = Buffer.byteLength('你好世界hello','utf8');
  console.log(len);//17
  ```

- 判断一个对象是否是Buffer对象

  ```js
  Buffer.isBuffer(obj);
  返回结果是 true / false
  ```

- 获取某个Buffer字节

  ```js
  buf[index]
  ```

  



### Nodejs中模块系统

##### 什么是模块化？

1.引入系统模块，如 path url express ... ， 如 var express  = require( ' express ' )

2.引入用户自定义的模块,如  var pah = require( ' ./xxx.js ' )

##### exports对象 

node系统自带对象 exports，一般用于引入其他js文件的时候，可以将其他文件中的方法属性赋值给exports

- exports 和 module.exports 的区别

  获取单一成员：module.exports  = xxx

  获取多个成员：exports.xx1 = xxx   或  module.exports = { }
  
- 所有被引入文件都是返回的module.exports  这个对象！默认情况下exports和module.exports都指向同一个对象！

```js
//使用方法
//1.被引入文件 b.js
router.get('/',...)
...
module.exports = router //把这个router对象赋值，那么引入文件就可以用了
//2.引入文件
var router = require('./b.js')
...
app.use(router)//这里app是服务器，使用router路由
```

```
//exports使用方法
//1.被引入文件 b.js
exports.save = function(){}
exports.find = function(){}
...

//2.引入文件 
var student = require('./b.js')
student.save() //这里就可以使用b.js文件中的方法啦
```

##### package.json package-lock.json

- package.json  存储包名、版本号等
- package-lock.json  加速npm包的下载的工具，npm5以上的版本自带的。里面保存了所有包的下载地址等信息，以后Install的时候更快。

##### 



### 使用模块

##### 模板引擎art-template

- 安装：npm i art-template

- 在html中怎么使用？

  ```javascript
  //引入art-template
  <script src= 'node_modules/art-template/lib/template-web.js' ></script>
  //注意：1.type这里面写template,不然浏览器会把它当做js去处理
  //2.要加 id
  <script type="text/template" id="tpl">
      <div>
          <label for="">用户名</label>
          <input type="text" value="{{ uer.username }}">
      </div>
      <div>
          <label for="">年龄</label>
          <input type="text" value="{{ uer.age }}">
      </div>
      <div>
          <label for="">职业</label>
          <select name="" id="">
              {{ each jobs }}
              {{ if uer.job== $value.id}}
              <option value="{{$value.id}}" selected>{{ $value.name }}</option>
              {{ else }}
              <option value="{{$value.id}}" >{{ $value.name }}</option>
              {{ /if }}
              {{ /each }}
          </select>
      </div>
  </script>
  <script src="node_modules/jquery/dist/jquery.js"></script>
  <script src="node_modules/art-template/lib/template-web.js"></script>
  <script>
      function pGet(url) {
          return  new Promise(function (resolve,reject) {
              var xhr = new XMLHttpRequest()
              xhr.open('get',url,true)
              xhr.onload=function () {
                  //加载成功后会执行下面
                  resolve(JSON.parse(xhr.responseText))
              }
              xhr.onerror=function (err) {
                  reject(err)
              }
              xhr.send()
          })
      }
      var data = {}
      pGet('./user.json')
          .then(function (userData) {
              var uer = userData.user[3]
              console.log(uer)
              data.uer = uer;
              return pGet('./jobs.json')
          })
          .then(function (jobData) {
              var jobs = jobData.jobs
              data.jobs = jobs
  //这里就是开始渲染引擎模板，参数1 是上面的id，参数2 是数据对象，记得要是对象形式
              var htmlStr = template('tpl',data) 
              document.querySelector('#user_form').innerHTML = htmlStr
          })
  ```

- 在node中怎么使用

  ```
  //使用场景1
  var template = require('art-template')
  var fs = require('fs')
  fs.readFile('./xxx.html',function(err,data){
  	...
  //这里其实和上面script中使用方法一致，都是参数1是 渲染的id文件 ，参数2 是对象数据 
  	var ret = template(data.toString(),{
  		name:'ck',
  		...
  	})
  	res.end(ret) 
  })
  
  ```

  ```
  //使用场景2
  ...
  app.engine('html',require('express-art-template')) //在node中配置template模板
  app.get('/',function(req,res){
  	...
  //这里是express框架提供的方法，参数1 直接渲染xxx.html文件，参数2 是对象数据
  	res.render('xxx.html',{comments:comments})
  })
  
  ```

  ##### URL模块

  ​	在原始http中，url模块是用来获取 form表单，用户get请求，拿数据的

  ```
  var http = require('http')
  var fs = require('fs')
  var url = require('url')
  http
  	.createServer(function(req,res){
  //1.原始http中的req.url得到的是 /abc?name:ck$$age:20 这样的请求路径加数据的形式
  //2.下面url.parse()方法后，得到的是obj对象，obj里面的pathname属性是请求路径，obj.qurey属性是 ? 后面的数据
  	
  //3.下面的方法参数2 记得加 true
  //4.
  		var obj = url.parse(req.url，true)
  		var pathname = obj.pathname
  		if(pahtname==='/'){...}
  	})
  	.listen(3000,...)
  
  ```

  ##### 重定向问题

  ```js
  //在原始Http中的重定向
  res.statusCode = 302
  res.setHeader('Location','/')//跳转到首页
  
  //在express模块中，可以直接重定向到首页
  res.redirect('/')
  
  //异步操作中，类似于有ajax加入的时候，不可以使用 res.redirect('/') 
  //要在ajax中直接 window.location.href =  '/'
      $('#register_form').on('submit',function (e) {
          e.preventDefault()
          var formData = $(this).serialize()
          $.ajax({
              url:'/register',
              type:'post',
              data:formData,
  
              dataType: 'json',
              success:function (data) {
  
                  var err_code = data.err_code
                  if (err_code === 0){
                      window.alert('注册成功!')
  //这里。
                      window.location.href='/'
                  }else  if (err_code ===1){
                      window.alert('Emai or nickname already exists.!')
  
                  }else if (err_code===500){
                      window.alert('保存失败!')
  
                  }
                  //console.log(data)
              }
          })
      })
  
  
  ```

  

### 要安装的模块

```
npm init -y //安装package.json文件
npm i express
npm i --global nodemon //安装全局nodemon-->nodemon ..js文件,自动刷新node文件的插件
npm i art-template express-art-template 
npm i body-parser //表单post提交的时候，req.body 得到？后面的数据
npm i path //用于配置静态文件，app.use(express.static(path.jion(__dirname,'/public')))
npm i mongoose //安装mongodb数据库包

npm i blueimp-md5 //用于给密码加密的 var md5 = require('blueimp-md5')  password: md5(md5(body.password))
```

### express框架

```shell
var express = require('express')
var path = require('path')
var bodyParser = require('body-parser')
var router = require('./router.js')


var app = express()

//配置art-template
app.engine('html',require('express-art-template'))

//配置body-parser
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())

//开放public文件中静态资源文件
app.use('/public',express.static(path.join(__dirname,'/public')))

//使用路由
app.use(router)

app.listen(3000,function(){})
```

##### res.end()  res.send() 区别

```js
1.参数类型区别，res.send()可以是BUffer、字符串、数组;res.end()只能是字符串和Buffer对象
2.res.send()内部自动生成了响应报文头，不会像end中不设置响应报文头会乱码！
```

##### app.get 和 app.use 和 app.all() 区别

```js
都是注册一个 请求/ 的路由
app.get('/' , function(req,res){})
app.post('/' , function(req,res){})

app.use('/index',function(req,res){});
1.路由请求中不限定请求方法；
2.请求路径中的第一部分只要与 /index/ 相等既可，并不要求请求路径和pathname一致！

app.all()
1.不限定请求方法，但是请求路径pathname必须一致！
```

##### req.params

- 作用：当请求路径不是?开始的时候，如 访问 index/2019/11/29，那么可以用req.params获取路径参数

  ```js
  app.get('/index/:year/:month/:day',function(req,res){
      res.send(req.params);//{"year":"2017","month":"11","day":"29"}
  })
  ```

##### 开放静态资源

```js
express提供了一个开放静态资源的方法
var fn = express.static(path.join(__dirname,'/public'))
app.use('/',fn);//表示用户在访问网站的时候，就可以开放静态资源直接访问到里面的开放文件！！！
```

##### express中res的方法

```js
res.redirect('https://ju11.net') //重定向

res.sendFile(path.join(__dirname,'/...'),function(err,data){  }) //读取文件
```



##### body-parser的配置

```shell
var bodyParser = require('body-parser')
//配置：
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))
// parse application/json
app.use(bodyParser.json())
```

##### art-template的配置

```shell
app.engine('html',require('express-art-template'))

//参数1 是可以直接渲染views文件中html文件，找views文件是系统自带的，也可以修改，如下：
app.set('views',xxx)//xxx为要修改的文件名
```

##### 模块之 nodemailer



##### 路由文件的使用

```shell
//被引入的路由文件 router.js
var express=  require('express')
var router = express.Router() //这里是express的方法

//这里的router相当于主文件中的服务器名app.
router.get('/',function(req,res){
	//这里面的方法，可以再开一个js文件，封装后，调用
})
...

module.exports = router //记得写这句，然后再主文件app.js中调用router.js即可
```

##### cookie 中间件的使用

和session不同的是，cookie是res.cookie,通过 cookieParser中间件，通过res去写cooike，前端才可以拿到

```js
--app.js文件中
//引入cookie中间件
var cookieParser = require('cookie-parser')
//配置
app.use(cookieParser());


--router.jS文件
router.post('',(req,res,next)=>{
  User.findOne({},(err,doc)=>{
    if(doc){
        //参数1 cookie的名称，参数2 cookie的值 ， 参数3 是对象，存放地址和存放周期
       res.cookie('userId',doc.userId,{
           path:'/',
           maxAge:1000*60*60
       }) 
       req.session.user=doc;//通过session时时拿到用户的信息！
   		
      res.json({
          ...
      })
      }
	})  
})
```



##### session的使用

​	session是用来记录登录的状态的,该插件会给req添加一个对象,req.session

1. 由于客户端http的请求是无状态的，不知道是谁在请求，所以1.登录的时候，发布一个加密字符串（包含用户相关信息）给前端。2.调用其他的接口的时候将加密字符串发给服务器，随身携带者字符串，3.服务器验证。
2. 用到模块：express-session    和    cookie-parser

```shell
安装： npm i express-session
配置：在npm官网搜 express-session
app.use(session({
//加密，字符串是随意写的
  secret: 'keyboard cat',
  cookie:{maxAge:60*1000*60*24*7},//设置过期时间7天，1000不知道是啥
  resave: true,//默认值是true,即使session没有修改，也保存session值
  saveUninitialized: false,//默认false,如果为true,就是每次请求ajax的时候都会再生成个session cookie  
}))
```

​	应用场景：当用户注册完或登录后，记录当前状态

​	req.session是对象，可以赋值一个属性用来记录用户登录或注册输入的数据，然后在主页面那里渲染到页面，显示登录状态就好了。如下：

```shell
//主页面
router.get('/',function(req,res){
	res.render('index.html',{
		user:req.session.user //调用了
	})
})

【 req.session.login =true;//在需要用到登录状态才可以操作的接口的时候，这个login写在登录的接口那里】


//用到session的
router.post('/register',(req,res,next)=>{
	if(req.session.login){
		next()
	}else{
	res.send({err:-999,msg:'先登录'});//前端拿到这个数据后，根据err跳转到登录界面即可！
	}
},function(req,res,next){ //这里记得加next
	
	...
	new User(req.body).save(function(err,user){
		if(err){
			return next(err)//这里next是直接主文件写的处理错误的中间件，
		}
		req.session.user = user //注册的时候记录状态，并记录用户输入的值，主页面可以调用
   
		res.status(200).json({ //这里是express自带的json方法，把对象转为字符串，给客户端的ajax拿
			err_code:200, 
			message:'OK'
		})
	})
	
})


//引入个客户端的ajax
//下面这个ajax是客户端去给服务端传递用户输入的数据，服务端判断好数据后，返回结果，然后渲染！
$.('#表单的id').on('submit',fucntion(e){ //这里必须是submit
	e.preventDefault()
    var formData = $(this).serialize() //这个方法是获取用户提交的数据的
    $.ajax({
    	url:'/register',
    	type:'post',
    	data:formData,
        dataType: 'json',
        success:function (data) {
                var err_code = data.err_code
                if (err_code === 0){
                    window.alert('注册成功!')
                    window.location.href='/'  //重定向
                }else  if (err_code ===1){
                    window.alert('Emai or nickname already exists.!')
                }else if (err_code===500){
                    window.alert('保存失败!')

                }
                //console.log(data)
            }
    })
})
```

- 注销session的方法

  ```js
  在退出登录的时候可以用到req.session中的方法 req.session.destroy();
  app.get('/logout',(req,res)=>{
      req.session.destroy();
      res.send({err:1,msg:'退出成功！'})
  })
  
  ```

- 注意：使用session的时候，不能跨域

##### 中间件的使用

- 含义：写一个js文件，包装一个函数，在主文件调用这个js文件，就可以实现某个方法。

中间件的本质就是app.use(  ) ,自定义中间件需要写在主文件的app.use(router) 后面 ，如下：

```shell
//写一个处理全局错误的中间件，这里是app.js文件
...
app.use(router)

//一个404中间件，必须在app.use(router)后面，意思是路由出错了，就跳转到这个页面
app.use(fucntion(req,res){
	res.render('404.html')
})

//全局错误中间件，在router.js文件中可以调用，可以参考上一个代码块的案例
app.use(fucntion(err,req,res,next){  //这里的err必须写，4个参数不可以漏掉
	res.status(500).json({
		err_code:500,
		message:err.message
	}) 
})

```

##### 在express中的中间件

- 中间件：处理请求的，本质就是函数

- 在express中不关心请求路径和请求方法的万能匹配中间件，也就是说任何请求都会进入这个中间件，中间件可以有多个。

  ```js
  var express = require('express');
  var app = express();
  //中间件本身是方法，改方法接收三个参数
  req 请求对象 ；res 响应对象 ； next 下一个中间件
  app.use(function(req,res,next){
      alert('1');
      next();//当一个请求进入一个中间件后，必须使用next()方法去调用下一个中间件，如果这个不加next，那么输出结果只会是 1 ,加了才会是 1  2 ；
  })
  app.use(function(req,res,next){
      alert('2');
    
  })
  app.listen('3000',function(){ console.log( 'Running...' ) })
  ```

- 以 /xxx 开头的路径中间件

  ```js
  1.next也要匹配的，不是调用紧挨着的那个! 比如用户请求 /a/b/c
  下面输出的  1  2
  app.use(function(req,res,next){
      alert('1');
      next();
  })
  app.use('/b' , function(req,res,next){
      alert('3');
  })
  app.use('/a' , function(req,res,next){
      alert('2');
  })
  ```

- 因为app.use( '  ' ) 是只要以 /xxx开头既可，还有2中严格匹配的中间件，app.get( ' / ' )  app.post(  )

- next( err )  当next里面传入err参数的时候，就是直接往后找带有err的4个参数的中间件！

##### 局部中间件

```js
app.get('/abc',function(req,res,next){
    ...
    next();
},function(req,res){
    
})

//格式 app.get('/xxx',fun,fun,fun...)
```





##### 更改views目录

```js
app.set(‘views’, path.join(__dirname, ‘xxx’));
```

##### res.json({ }) 和 res.send()区别

```js
返回json格式的数据的时候推荐使用 res.json()
二者在返回数组和对象的时候是没有区别的

里面的内容就是，ajax  success:function(data){} 中的data数据！
```





### 数据库mongodb

安装：

​	1.下载好后，复制 mongdb/.../bin 一直到bin目录，然后点击此电脑-属性-高级系统设置-环境变量-path-新建（把刚才的复制进去）

​	2.启动数据库，cmd 输入  mongod （本地服务器中自动启动数据库（services.msc）的时候，不需要启动数据库）

​	3.链接数据库，cmd 输入 mongo 

- 注意：：本地C盘 建立data/db  文件夹用来存数据库的数据！！！！！！



文档：

##### 1、cmd中数据库的使用

```shell
启动 mongod
链接 mongo
基本命令：
show dbs 查看显示所有的数据库
db 查看当前操作的数据库
use 数据库名称 切换到指定的数据库（如果没有会新建）如： use itcase
show collections 查看集合
在use itcase数据库名后，插入数据 db.students.insertOne({ "name" : "jack" })
students  是集合名，show collections 可以查看所有集合名

可以把students看成数组，插入的数据都是对象！
db.students.find() --可以输出里面的数据
	{//itcases 是数据库名,最大的， students是集合名，下面的{}是表单，
		itcase:{
			students:[
			{},
			{},
			{}
			]
		},
		itcase2:{
			data:[
			{},
			{},
			{}
			]
		}
	}
建库: use + 库名 --db.集合名.insertOne({   })

2. db.students.findOne() 查询第一条数据
3. db.students.update() 更新数据
{ "_id" : ObjectId("5e1c40d32d84aaf173268720"), "name" : "aaaa", "age" : 20, "id" : 1 }
{ "_id" : ObjectId("5e1c413d2d84aaf173268721"), "name" : "zzzp", "age" : 30, "id" : 2 }
更新方法：先查询再更新
db.user.update({"age":20},{$set:{"name":"aaaa"}})

根据年大小查询数据！
db.students.find({"age":{$gt:20}});//$gt 是 > 号 ； $lt 是 < 号，$eq 是  = 号 

删除数据 db.user.remove({"age":20})
删除集合 db.user.drop()

db.createCollection("user") 创建集合
```

###### 1-1mongodb创建用户

为了数据库的安全性，必须要创建用户！

***数据库的可视化工具*** Robo-3T

创建数据库

```js
db.createUser(
{
 user: "xxx",
 pwd: "xxx",
 roles: [{role: "readWrite", db: "peper_test"}]
 }
)
```







######  

##### 2、node中mongoose的使用

先下载npm i mongoose 

```shell
//被引入文件 Schema.js
//要先设计Schema模板
var mongoose = require('mongoose')
var Schema = mongoose.Schema //创建Schema构造函数

mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true});
mongoose.connection.on("connected",function(){
	console.log("MongoDB connected success")
})
mongoose.connection.on("error",function(){
	console.log("MongoDB connected fail")
})
mongoose.connection.on("disconnected",function(){
	console.log("MongoDB connected disconnected")
})

//设计文档数据结构（就是表数据格式，类似于上面students里面的{}）
var userSchema = new Schema({
  email: {
    type: String,
    required: true
  },
  nickname: {
    type: String,
    required: true
  },
  password: {
    type: String,
    required: true
  },
  created_time: {
    type: Date,
    // 注意：这里不要写 Date.now() 因为会即刻调用
    // 这里直接给了一个方法：Date.now
    // 当你去 new Model 的时候，如果你没有传递 create_time ，则 mongoose 就会调用 default 指定的Date.now 方法，使用其返回值作为默认值
    default: Date.now
  },
  last_modified_time: {
    type: Date,
    default: Date.now
  },
  avatar: {
    type: String,
    default: '/public/img/avatar-default.png'
  },
  bio: {
    type: String,
    default: ''
  },
  gender: {
    type: Number,
    enum: [-1, 0, 1],
    default: -1
  },
  birthday: {
    type: Date
  },
  status: {
    type: Number,
    // 0 没有权限限制
    // 1 不可以评论
    // 2 不可以登录
    enum: [0, 1, 2],
    default: 0
  }
})


//将文档发布成model
//下面的User就是集合名，被引入Node文件生成数据后，就变成复数了，uers
var User = mongoose.model('User',userSchema)
module.exports = User;
```

##### 3、mongoose中的方法

- 在mongoose网址中API方法->Model里面有操作方法

```shell
var User = require('./Schema.js') //引入模块

--------
查询
--------
//查询所有
// User.find(function(err,ret){
// 	if (err) {
// 		console.log('查询失败')
// 	}else{
// 		console.log(ret)
// 	}
// })
//条件查询
// User.find({
// 	username:'zhangzp16'
// },function(err,ret){
// 	if(err) {
// 		console.log('查询失败')
// 	}else{
// 		console.log(ret)
// 	}
// })
//查询一个
// User.findOne({
// 	password:'123'
// },function(err,ret){
// 	if (err) {
// 		console.log('查询失败')
// 	}else{
// 		console.log(ret)
// 	}
// })
//通过id查询
User.findById(req.body.id,function(){})

//条件查询  https://blog.csdn.net/xiongzaiabc/article/details/81186998
1.正则关键字查询
router.post('/',(req,res)=>{
	let {keyWords} =req.body;
	let reg = new RegExp(keyWords);
	//$gte $or $and $regex
	User.find({age:{$gte:16}})
			.then()
			.catch()
    User.find({$or:[{name:{$regex:reg}},{decsd:{$regex:reg}}]})
			.then()
			.catch()
})

//通过分页查询
router.post('/',(req,res)=>{
	let pageSize = req.body.pageSize || 5;
	let page = req.body.page || 1;
	
	User.find().limit(Number(pageSize)).skip(Number(page-1)*pageSize)
			.then()
			.catch()

}
---------
保存增加
---------
new User(req.body).save(function(){})


---------
修改更新
---------
//这里是通过id查，也可以通过username查
User.findByIdAndUpdate('5da5ae1c61a03f20341cb27f',{
	password:'456'
},function(err,ret){
	if (err) {
	console.log('更新失败')	
	}else{
	console.log('更新成功')
	console.log(ret)
	}
})

//通过查询后 原始方法修改 然后进行保存
User.findOne({user})

//User.update()方法
  1.参数1 是按条件查询  2.参数2记得加 $ 链接 此处的$ === $set 3.参数3是回调
  User.update(
    {"userId":userId,"cartList._id":_id},
    {"cartList.$.productNum":productNum,
    "cartList.$.checked":checked},
    fn
  })
---------
删除
---------
// User.remove({
// 	username:'zhangzp16'
// },function(err,ret){
// 	if(err) {
// 		console.log('查询失败')
// 	}else{
// 		console.log(ret)
// 	}
// })

.findByIdAndRemove (function(){}) --通过查找id，删除数据

--通过id，删除几个
User.remove({_id:[id,id,id],()=>{})


//删除
User.remove({
 	username:'zhangzp16'
})
	.then(()=>{ res.send({err:0,msg:'remove success'}) })
	.catch(()=>{ res.send({err:-1,msg:'failed'}) })

都可以用.then() 方法去写！
```

###### 3-1sort 排序

```js
// 正序
Model.find()
    .sort({name:1})
    .exec(callback);
// 倒序
Model.find()
    .sort({name:-1})
    .exec(callback);
```





### Promise 容器

一般我们有时候需要通过多个接口获取数据，用ajax的时候都是嵌套，现在可以用promise容器

例子：比如要获取a.json文件 和  b.json文件中的数据

```shell
//引入jquery文件，jquery中是兼容了promise容器功能
//封装ajax函数
function get(url,callback){
	var xhr = new XMLHTTPRequest()
	xhr.open('get',url,true)
	xhr.send()
	xhr.onload = fucntion(){
		callback(JOSN.parse(xhr.responseText))//得到的是字符串，要先转化成对象才好在模板引擎中渲染
	}
}

//第一种方法：用jQuery
var data = {}
$.get('./a.json') //jquery中不需要传入回调，下面的data就是拿到的数据
	.then(fucntion(adata){
		data.aobj = adata
		return $.get('./b.json') //return  第二个ajax，下面会收到
	})
	.then(fucntion(bdata){
		data.bobj = bdata
		var tmp = template(id,data)
		document.querySelector('#...').innerHtml = tmp //渲染页面
	})

//第二种方法：promise
//1.封装promise的ajax函数
function pGet(url,callback){
	return new Promise(function(resolve,reject){
		var xhr = new XMLHTTPRequest()
		xhr.open('get',url,true)
		xhr.send()
		xhr.onload = function(){
			//callback&&callback(JSON.parse(xhr.responseText))
			resolve(JSON.parse(xhr.responseText) //成功了就输出这个
		}
		xhr.onerror = function(){
			reject(err)
		}
	})
}

var data = {}
pGet('./a.json') //jquery中不需要传入回调，下面的data就是拿到的数据
	.then(fucntion(adata){
		data.aobj = adata
		return pGet('./b.json') 
	})
	.then(fucntion(bdata){
		data.bobj = bdata
		var tmp = template(id,data)
		document.querySelector('#...').innerHtml = tmp //渲染页面
	})
```

- 补充一点，mongoose中的 .find(  )

##### 案列总结

```js
1.数据库是用来存储用户的数据，以及拿取做判断，要引用到使用数据的那个js文件中！
	数据库要设计用户输入的模板！也就是Schema!
2.ajax是提交表单数据的，后台根据提交过来的数据，到数据库找数据然后做出判断，给客户端返回结果！
	怎么判断：1.到数据库找，怎么找？怎么保存？
			2.找到后怎么判断？怎么给客户端返回？/res.status(200).json({}),json方法是express提供的，给客户端返回字符串形式，jquery会自动转为对象的， sucess:function(dada){}，data就是返回的{}里面的值！
```

```js
--学生添加案例--
用到的技术点：
1.var ret = arr.find(function(item){
    return item.id === id   //数组里面的{}id值一样时候，返回这个{}
})
var index = arr.findIndex(function(item){
    return item.id === id   //数组里面的{}id值一样时候，返回这个{}的索引
})
2.JSON.parse(字符串) -- 转为对象 ； JSON.Stringify(对象) --字符串 ; 
	JSON.Stringify({student:student}) --保存形式注意！
3.fs.writeFile(写入的文件名，写入的数据，function(){})
4. Schema 的写法
```

```js
--Blog案例--
首先主要是客户端通过ajax向服务端传递数据，这个ajax怎么传的是重点！！！
用到的技术点：
1. ajax技术
2. 给密码加密 var md5 = md5(md5(password)) // npm i blueimp-md5
3. session 
```



### Ajax

```js
1.form表单提交数据，在提交页面写入ajax，通过ajax提交数据！不用表单提交，表单提交数据，没有返回，如果需要返回服务端处理的结果需要重新渲染页面！
2.服务端拿到ajax提交来得数据后进行判断，然后 res.json({})给客户端，
```

```js
思考：
有一种页面是 直接点击选项中的按钮，后面的信息就会跳出来，这种是不是ajax做的？

```

### 浏览器渲染引擎工作原理

```js
1.解析html构建DOM树
2.构建“渲染树”
3.对渲染树进行布局，也就是layout或reflow，就是定位啊、显示、隐藏啊等
4.绘制“渲染树”，调用系统API进行绘图操作！

补充：对于layout的面试题，创建50个文本框，然后添加到div中，怎么做？
如果是直接for循环做，那么没添加一次就要layout布局一次，性能不好。
可以把50个文本框先添加到文档片段 createDocumentFragment中！再添加到div中
exp:
var div = document.getElementById("dv");
var k = document.createDocumentFragment();
var p = document.createElement("p");
k.appendChild(p);//添加到文档片段中
div.appendChild(k)//再从文档片段中添加到div
```

### php和Nodejs区别

```js
用户发8080端口请求，apache服务器监听8080端口，接收到请求后，判断是静态文件直接读取返回客户端，是动态文件给PHP处理，然后再返回客户端！

nodejs本身就是一个服务器不需要apache了，nodejs监听8080端口，接收到请求，无论静态还是动态直接交给Nodejs处理！
```

### 爬虫案例

1. 获取目标网站：http.get()方法，nodejs逛网查找http.get

2. 分析网站内容：cheerio插件  ，用此插件可以使用jquery里面的方法

3. 获取有效信息 下载或者其他操作

   ```js
   var http = require('https');
   var url = 'https://www.bilibili.com/';
   http.get(url, (res) => {
       const {statusCode} = res;
       const contentType = res.headers['content-type'];
       let error = null;
       if (statusCode !== 200) {
           error = new Error('请求失败' + `${statusCode}`);
       } else if (!/^text\/html/.test(contentType)) {
           error = new Error('请求类型错误');
       }
       if (error) {
           console.log(error);
           //释放缓存
           res.resume();
           return;
       }
   
       let rawData = '';
       res.on('data', (chunk) => {
           rawData += chunk.toString('utf8');
       });
       res.on('end', () => {
           //引入cheerio 插件
           const cheerio = require('cheerio');
           const $ = cheerio.load(rawData);//将请求的网页转化
           $('img').each((index, item) => {
               console.log($(item).attr('src'));
   
           });
           console.log('数据传输完毕!')
   
       })
   
   
   }).on('error', (e) => {
       console.error(`出现错误: ${e.message}`);
   });
   ```

### 发送邮箱验证码

- 邮箱js文件 参考：<https://www.cnblogs.com/wuyepeng/p/10081677.html>

```js
'use strict';

const nodemailer = require('nodemailer');

let transporter = nodemailer.createTransport({
    // host: 'smtp.ethereal.email',
    service: 'qq', // 使用了内置传输发送邮件 查看支持列表：https://nodemailer.com/smtp/well-known/
    port: 465, // SMTP 端口
    secureConnection: true, // 使用了 SSL
    auth: {
        user: '599357050@qq.com',//你的邮箱
        // 这里密码不是qq密码，是你设置的smtp授权码
        pass: 'kvsbkpbqappwbcaf',
    }
});

function send(mail, code) {
    let mailOptions = {
        from: '"张志鹏" <599357050@qq.com>', // 这里写上你自己的邮箱
        to: mail, // 这里写上要发送到的邮箱
        subject: '验证码', // Subject line
        text: `验证码为：${code} 5分钟内有效`
        // html: '<b>验证码为：456786 5分钟内有效</b>' // html body
    };
//在主文件调用的时候，需要判断是否发送成功,但是又不能直接res.send()，因为是异步，所以这里可以
    //用Promise封装。
    return new Promise(function (resolve,reject) {
        transporter.sendMail(mailOptions, (error, info) => {
            if (error) {
                reject(error)
            }else{
                resolve()
            }
        });
    });

}

module.exports = {send};

```

- 主文件

```js
var http = require('http');
var Mail = require('./03-email');
http.createServer(function (req,res) {
    if (req.url==='/'){
        var mail = '2583083204@qq.com';
        var code = parseInt(Math.random()*1000);
       Mail.send(mail,code)
           .then(()=>{
               res.setHeader('Content-Type','text/plain; charset=utf-8')
               res.end('成功！')
           })
           .catch((err)=>{
               res.setHeader('Content-Type','text/plain; charset=utf-8')
               res.end('失败！')
               console.log(err)
           })
    }
}).listen(3000,function () {
    console.log('running...')
});
```

### apidoc文档生成

1. 安装：`npm i apidoc -g`

2. 官网：`<https://apidocjs.com/>`

3. 方法：在项目目录`demo`下创建一个文件夹`api`，cmd cd到demo文件夹，然后`apidoc -i ./ -o ./api`

   -->   apidoc -i 当前目录 -o 创建的文件夹

   ```js
   1.如下代码写到每一个router上面
   
   /**     //{ 提交方式 }  /数据库名/接口名    接口描述
    * @api {get} /user/login    id Request User information
    * @apiName GetUser  //接口名字 可以和上面一致 login
    * @apiGroup User  //分组，就写当前数据库名字
    *   //下面都是参数的描述！！！
    * @apiParam {Number} id Users unique ID.
    *  
    * @apiSuccess {String} username Firstname of the User.
    * @apiSuccess {String} password  Lastname of the User.
    */
   ```

4. 上述写完了后在`demo`文件夹中创建一个 `apidoc.json`文件

   ```js
   {
     "name": "example",
     "version": "0.1.0",
     "description": "apiDoc basic example",
     "title": "Custom apiDoc browser title",
     "url" : "https://127.0.0.1:3000"
   }
   ```

### 跨域问题

- 跨域就是在域名、协议、端口不同的时候就会无法访问，开发中，我要请求后端的服务器，那么会出现跨域问题。可以自己访问自己的服务器，然后通过一个 `request`中间模块去请求后端的服务器，就解决了跨域问题。

- 实际开发中只有这一种方法不需要后端人员去处理！

- 要先安装request模块

   ```js
  1.html页面的ajax请求先访问自己的服务器，然后在自己的服务接口中，request(url,fn)方法到后端的服务器
  ------
  html
  $.get('http://localhost:3000/abc',{},()=>{})
  ------
  
  2.node
  app.get('http://localhost:3000/abc',(req,res)=>{
      request方法帮忙访问后端服务器
      requset('后端url'，(err,response,body)=>{
          if(!err){
               res.send(body)//body就是拿到的数据
          }
         
      })
  })
  ```
  
  

### 长连接(socket)和短连接(ajax)

- 长连接就是时时保持更新的！
- 实现长连接的几种方式：
  - net
  - socket.io 麻烦 兼容性好
  - websocket  H5新增属性 低版本不兼容

```js
前后端连接
1.搭建 socket 服务器， new Websocket.server({prot:8080},()=>{})
	ws.on('connection')
2.前端进行连接  new Websocket('ws://localsocket:8080')
	ws.onOpen()
数据交互
	client.on('message',()=>{})
3.前端主动发送数据
4.后端主动发送数据
	wx.onmessage=()=>{}
    wx.send()
前后端断开的处理
ws.on('close')
ws.onClose()
5.断开连接

```





### 小知识点

##### 1.查询本地电脑ip

- windows：cmd输入  ipconfig     mac ifconfig

  

  