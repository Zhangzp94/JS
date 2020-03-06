

### Webpack

- 安装：

  ```js
  全局：npm i webpack  npm i webpack-cli
  项目：npm i webpack -D  npm i webpack-cli -D
  ```

  

- 作用：`webpack`可以处理js的兼容问题，把高级的浏览器不识别的语法转为低级的！
- 运行的命令格式：`webpack  要打包的文件路径   打包好要保存的文件路径`
- 安装`webpack` ：`npm i webpack -g`  或者 安装到项目目录中 `npm i webpack --save-dev`

##### （1）打包文件命令

`webpack ./src/main.js -o ./dist/bundle.js --mode development`

`webpack 要打包的文件  -o 打包后保存的文件地址  --mode development`

##### （2）`webpack-dev-server`

- 这个工具是用来帮忙自动打包编译的功能，并且自动刷新浏览器！

- 安装：`npm i webpack-dev-server -D`

  注意1：由于`webpack-dev-server`只能安装在本地目录项下面，所以不能直接`cmd`运行`webpack-dev-server`，所以可以通过`npm init -y`创建的`package.json`文件中的`"scripts"`属性创建一个执行命令！

```js
--package.json文件
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server"
  },
      
      
```

​	注意2：如果想要`webpack-dev-server`正常运行，那么需要在本地项目中再次安装`webpack`，`npm i webpack -D`

​	运行:  `webpack-dev-server:`  `npm run dev`

````js
--npm run dev 后cmd中的显示代码！！！

i ｢wds｣: webpack output is served from /  输出的bundle.js文件在根目录下

系统自动把编译后的bundle.js文件保存在电脑内存中，不是磁盘中的./dist/bundle.js文件，所以页面渲染的时候
也是访问的是内存中的/bundle.js文件！此时需要把编译的那个文件中的
 <script src="../dist/bundle.js"></script>
修改成
 <script src="/bundle.js"></script>
````

***`webpack-dev-server指令`***

````js
--package.json文件
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --open --port 3000 --contentBase src --hot"
  },
  --open 修改完文件后，自动打开页面
  --port 3000 修改端口号
  --contentBase src 让打开的页面直接打开src这个文件
  --hot 让修改后的页面不重新刷新，这是加载个修改的补丁，局部刷新
````

*上面是一种简单的方法，下面是复杂点的方法！*

```js
--webpack.config.js文件
var webpack = require('webpack');//热更新第二步
devServer:{
    open:true,//自动打开页面
    port:3000,//设置端口号
    contentBase:'src',//默认打开的文件
    hot:true//热更新第一步，热更新就是补丁更新不刷新页面
  },
  plugins:[
      new webpack.HotModuleReplacementPlugin()//热更新第三步
  ]
```

##### （3）`html-webpack-plugin插件的使用`

````js
1.安装：npm i html-webpack-plugin -D 
2.作用：让index.html文件引入的bundle.js是从虚拟的内存中引用，而不是从本地磁盘中引用！
--webpack.config.js文件中
var htmlWebpackPlugin = require('html-webpack-plugin');
plugins:[ //配置插件的
      new webpack.HotModuleReplacementPlugin(),//热更新第三步
      /*
      * htmlWebpackPlugin 插件的2个作用！
      * 1.在内存中创建一个html文件
      * 2.不需要手动添加bundle.js文件了，这个插件会帮我们把打包好的bundle.js文件自动插入这个html文件中！
      * */
      new htmlWebpackPlugin({
        template:path.join(__dirname+'/src/index.html'),//指定 模本页面，会根据页面路径生成内存中的html
        filename: 'index.html',//生成的页面名称
        chunks:['index']//块，会使打包出来的文件就像没打包一样的分布
      })
  ]
````



##### （4）loader配置处理

- webpack默认只能处理js文件，如果要处理非js文件需要安装第三方 loader 加载器

- 安装：`npm i style-loader css-loader -D`

  - 然后打开`webpack.config.js`文件，新增一个配置节点，module，是一个对象，在这个对象上有一个

    属性rules，这个数组中存放了所有第三方文件的匹配处理规则！

  ```js
  --webpack.config.js文件中
  module:{//这个节点用于配置所有第三方模块，加载器
      rules:[//匹配规则
        {test:/\.css$/ , use:['style-loader','css-loader']}
      ]
    }
  
  如果需要解析less文件！
  安装：npm i less-loader -D
  module:{//这个节点用于配置所有第三方模块，加载器
      rules:[//匹配规则
        {test:/\.css$/ , use:['style-loader','css-loader']},
          {test:/\.less$/,use:['style-loader','css-loader','']}
      ]
    }
  ```

  

##### （5）webpack小总结

```js
1.原始webpack打包：webpack 要打包的文件 打包后保存的文件地址
2.自动编译打包：webpack-dev-server ,在package.json文件中添加"dev": "webpack-dev-server 
//自动打开页面  设置端口号  设置打开的页面  热更新（局部补丁更新不需要是刷新页面）
--open --port 3000 --contentBase src --hot"

在webpack.config.js文件中配置 入口和出口的文件，
module.exporst={
    entry:path.join(__dirname+'/src/main.js'),
    output:{
        path:path.join(__dirname+'./dist/'),
        filename:'bundle.js'
    }
}
--
3.在内存中根据指定的模板页面生成一份内存中的html页面，同时自动将bundle.js文件自动导入这份文件中，需要先
安装一个插件！html-webpack-plugin -D ，并且require到webpack.config.js文件中！
plugins:[
    new htmlWebpackPlugin({
        template:path.join(__dirname+'/src/index.html'),
        filename:'index.html'
    })
]
4.处理css less等各种文件的类型，因为webpack默认只能处理js文件！
安装：npm i style-loader css-loader less-loader -D
module:{//配置所有第三方配置文件的
    rules:[
        {test:/\.css$/ , use:['style-loader','css-loader']},
        {test:/\.css$/ , use:['style-loader','css-loader','less-loader']}
    ]
}
```

##### （6）url-loader

- webpack不能解析后缀.jpg的图片，所以需要安装`npm i url-loader file-loader`file-loader是依赖url的项！

```js
module:{//这个节点用于配置所有第三方模块，加载器
    rules:[//匹配规则
      {test:/\.css$/ , use:['style-loader','css-loader']},
      {test:/\.jpg$/,use:'url-loader?limit=100&name=[hash:8]-[name].[ext]'}
      /*
      * limit 是图片的字节大小，如果设置的图片字节大小 小于图片的实际大小，图片url就不会自动生成
      * Base64格式的字符串！
      * 1.当css文件中，有2个同名的image的时候，这是解析的时候后面的图片会替换前面的图片，页面显示的都是
      * 后面的那个图片，那么此时可以加个[hash:8]值，hash值可以接受32位，这里写8就好了！
      * name=[name].[ext] 表示生成的图片的名字和图片文件名一致！
      * */
    ]
  }
```

- `url-loader`也可以处理字体文件，比如bootstrap的字体文件！

  ```js
  import 'bootstrap/dist/css/bootstrap.css'
  module:{//这个节点用于配置所有第三方模块，加载器
      rules:[//匹配规则
        {test:/\.(eot|svg|ttf|woff|woff2)$/,use:'url-loader'}
      ]
    }
  ```

##### （7）babel配置

​	在主js文件中，我们使用了一些es6语法，此时需要使用第三方loader 把高级语法转为低级语法后，会把结果打包给webpack去打包到bundle.js文件中！

1. 在webpack中，可以运行如下两套包，去安装Babel 相关的loader功能

````js
第一套包：Babel的转换工具
npm i -D @babel/core babel-loader 
npm install --save-dev @babel/plugin-proposal-object-rest-spread
npm install --save-dev @babel/plugin-transform-runtime
npm install --save @babel/runtime
第二套包：Babel的语法
npm i @babel/preset-react @babel/preset-env babel-preset-mobx
````



2. 打开webpack.config.js文件，在Module节点下的rules数组中，添加一个新的匹配规则，

   `{test:/\.js$/,use:'babel-loader',exclude:/node_modules/}`

   注意：在配置babel的loader规则的时候，必须把node_modules排除在外，如果不排除则babel会把node_modules中的js文件全部打包编译，消耗cpu。

3. 在项目的根目录中，新建一个 `.babelrc`的babel配置文件，这个文件数据JSON文件，所以都需要满足Json语法!不能写注释！

   配置如下：

   ```js
   presets//预设--就是babel的语法
   {
       "presets": ["@babel/preset-env", "@babel/preset-react", "mobx"],
       "plugins": [
           "@babel/plugin-proposal-object-rest-spread",
           "@babel/plugin-transform-runtime"
       ]
   }
   ```

#### webpack文件的总结

```js
--webpack.config.js文件
var path = require('path');
var webpack = require('webpack');//热更新第二步
var htmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    //入口文件
  entry:path.join(__dirname+'/src/main.js'),
  //出口文件
    output:{
      path:path.join(__dirname+'./dist'),
        filename:'bundle.js'
    },
  devServer:{
    open:true,//自动打开页面
    port:3000,//设置端口号
    // contentBase:'src',//默认打开的文件
    hot:true//热更新第一步，热更新就是补丁更新不刷新页面
  },
  plugins:[ //配置插件的
      new webpack.HotModuleReplacementPlugin(),//热更新第三步
      /*
      * htmlWebpackPlugin 插件的2个作用！
      * 1.在内存中创建一个html文件
      * 2.不需要手动添加bundle.js文件了，这个插件会帮我们把打包好的bundle.js文件自动插入这个html文件中！
      * */
      new htmlWebpackPlugin({
        template:path.join(__dirname+'/src/index.html'),//指定 模本页面，会根据页面路径生成内存中的html
        filename:'index.html',//生成的页面名称
      })
  ],
  module:{//这个节点用于配置所有第三方模块，加载器
    rules:[//匹配规则
      {test:/\.css$/ , use:['style-loader','css-loader']},
      {test:/\.jpg$/,use:'url-loader?limit=100&name=[hash:8]-[name].[ext]'},
      /*
      * limit 是图片的字节大小，如果设置的图片字节大小 小于图片的实际大小，图片url就不会自动生成
      * Base64格式的字符串！
      * 1.当css文件中，有2个同名的image的时候，这是解析的时候后面的图片会替换前面的图片，页面显示的都是
      * 后面的那个图片，那么此时可以加个[hash:8]值，hash值可以接受32位，这里写8就好了！
      * name=[name].[ext] 表示生成的图片的名字和图片文件名一致！
      * */
      {test:/\.(eot|svg|ttf|woff|woff2)$/,use:'url-loader'},
      {test:/\.js$/,use:'babel-loader',exclude:/node_modules/}
    ]
  }
};
```

````js
--webage.json文件
 "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --open --port 3000 --contentBase src --hot"
  },
````

##### （8）在webpack中使用Vue

1.安装`npm i vue -s`

2.在js文件中直接导入

```js
第一种方法：
--main.js文件
import Vue from '../node_modules/vue/dist/vue.js'

第二种方法：
--main.js文件
import Vue from 'vue'

--webpack.config.js文件
resolve:{
    alias:{//修改vue被导入包的路径
   	 "vue$":"vue/dist/vue.js"
    }
}
```

***vue-loader***

```js
webpack 解析 后缀 .vue文件的loader
npm i vue-loader vue-template-complier -D
{test:/\.vue$/,use:'vue-loader'}
假如vue文件中，有scss样式，那么就需要添加能识别的loader,
{test:/\.vue$/,use:'vue-loader',options:{ loaders:{ scss:'style-loader!css-loader!sass-loader' } }}

style-loader!css-loader!sass-loader  是从右边向左边解析的！
```

***总结在webpack中使用vue***

```
1.安装vue包：npm i vue -s
2.webpack中推荐使用 .vue这个组件模板定义组件，所以安装能解析这种文件的loader。npm i vue-loader vue-template-compiler -D
记得在webpack.config.js文件中加入配置！{test:/\.vue$/,use:'vue-loader'}
还有加入
var htmlWebpackPlugin = require('html-webpack-plugin');
plugins:[
	new VueLoaderPlugin();
]

3.在main.js文件中导入 vue 模块， import Vue from 'vue'
4.定义一个 .vue文件创建组件
5.在main.js文件中 引入组件 import login from './login.vue'
6.在Index.html文件中创建<div id="app"></div>展示组件！
```

##### （9）`export default`和`export`区别

两者都是向外暴露对象使用，属于es6语法。

1.exports default 向外暴露成员，可以使用任意变量来接收，但是只能暴露一个变量

2.使用 export 可以暴露多个变量，但是接收的时候需要使用 { } 接收，使用export接收变量的时候，接收名必须严格按照导出的名称保持一致，如果实在要改名，可以用`{变量名 as 变量名2}`

```
--向外暴露的 test.js 文件
export default {
    name:'andy',age:20
}

export var a = {
    name:'ck'
}
export var b = {
    name:'zzp'
}

--接收的js文件
import info , {a as ab ,b} from './test.js'
console.log(info);
console.log(ab);
console.log(b);
```

##### （10）webpack中使用vue-router

1.安装vue-router: npm i vue-router

```
import VueRouter from 'vue-router'
Vue.use(VueRouter);//手动安装vueRouter


import Vue from 'vue'
import app from './app.vue'
import VueRouter from 'vue-router'
import login from './main/login.vue'
import register from './main/register.vue'
import a from './de/a.vue'
import b from './de/b.vue'
Vue.use(VueRouter);//手动安装vueRouter
var router = new VueRouter({
   routes:[
       {path:'/login',component:login,
        children:[
            {path:'a',component:a},
            {path:'b',component:b},
        ]
       },
       {path:'/register',component:register},
   ]
});
var vm = new Vue({
    el:'#app',
    render:c=>c(app),//渲染组件！
    router,
});
```

##### （11）组件中的lang 属性和 scoped属性

```
--组件.vue文件！
<style lang="scss" scoped>
    /*
    如果下面的样式用less的写法，就需要加lang="less"属性！
    如果只是当前的组件设置样式，就要加 scoped
    */
    body{
      div{
          color: red;
          font-style: italic;
          font-size: 40px;
        }
    }

</style>
```

##### （12）使用Vue实例render方法渲染组件

```js
<script>
    var login  = {
        template:'<h3>登录组件</h3>'
    };
    var vm = new Vue({
        el:'#app',
        data:{},
        methods:{},
        component:{},
        //createElements 是一个方法，调用它，能够把指定的组件模板渲染为html结构。
        //render方法渲染组件和传统的方法区别在于，render类似于v-text，会覆盖之前的元素，
        //并且使用render后 <div id="app"></div> 被覆盖了！
        render:function (createElements) {
            return createElements(login)
        }
       
    })
</script>
```

### 自己发布npm插件

1. npm init -y 创建package.json文件

2. 建立index入口文件，vue插件有一个原则必须要定义一个install方法，才能通过Vue.use（）方法

````js
开发vue插件基本的格式
import Toast from './Toast.vue'
let Toast = {}
Toast.install =function(){
    
}
//注意：通过webpack打包出来的vue文件是个模块，必须通过Vue.use()使用，那么在自己的vue插件中的最后加入
if(window.Vue){
    Vue.use(Toast)
}
export default Toast;//导出插件 相当于 export default { install() }
````







### 打包插件

***先安装webpack***

```js
npm i webpack -D  npm i webpack-cli -D
```

##### 1.vue文件插件

```js
cnpm i vue-loader vue-template-complier -D


注意点：会报错
但是解析vue文件的时候需要在 webpack.config.js文件中加

const VueLoaderPlugin = require('vue-loader/lib/plugin');
    plugins: [
        // make sure to include the plugin for the magic
        new VueLoaderPlugin()
    ],
```



```js
{test:/\.vue$/,use:'vue-loader'}
假如vue文件中，有scss样式，那么就需要添加能识别的loader,
{test:/\.vue$/,use:'vue-loader',options:{ loaders:{ scss:'style-loader!css-loader!sass-loader' } }}

style-loader!css-loader!sass-loader  是从右边向左边解析的！这里的loader需要自己先安装好
```

##### 2.babel插件

***作用：***在主js文件中，我们使用了一些es6语法，此时需要使用第三方loader 把高级语法转为低级语法后，会把结果打包给webpack去打包到bundle.js文件中！通过以下三步安装！

1. 在webpack中，可以运行如下两套包，去安装Babel 相关的loader功能

```js
第一套包：Babel的转换工具
cnpm i -D @babel/core babel-loader 
cnpm install --save-dev @babel/plugin-proposal-object-rest-spread
cnpm install --save-dev @babel/plugin-transform-runtime
cnpm install --save @babel/runtime
第二套包：Babel的语法
cnpm i @babel/preset-react @babel/preset-env babel-preset-mobx
```

2. 打开webpack.config.js文件，在Module节点下的rules数组中，添加一个新的匹配规则，

   `{test:/\.js$/,use:'babel-loader',exclude:/node_modules/}`

   注意：在配置babel的loader规则的时候，必须把node_modules排除在外，如果不排除则babel会把node_modules中的js文件全部打包编译，消耗cpu。

3. 在项目的根目录中，新建一个 `.babelrc`的babel配置文件，这个文件数据JSON文件，所以都需要满足Json语法!不能写注释！

   配置如下：presets//预设--就是babel的语法

   ```js
   {
       "presets": ["@babel/preset-env", "@babel/preset-react", "mobx"],
       "plugins": [
           "@babel/plugin-proposal-object-rest-spread",
           "@babel/plugin-transform-runtime"
       ]
   }
   ```

##### 3、css文件、sass文件

```js
cnpm i style-loader css-loader -D
cnpm i sass-loader  node-sass -D

{
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
}

{
        test: /\.scss$/,
        use: ['style-loader', 'css-loader','sass-loader']
},
```

##### 4、如下报错的解决方案

```js
在webpack.config.js文件中记得加  mode:'development'
不然会报错，内容如下

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/
```

### 发布npm

把自己的插件webpack打包好后，要想发布到npm上，需要在webpack.config.js文件下加如下代码

libraryTarget ：Js规范有amd umd commonjs ，amd只能用require， commonjs 只能用import，限制了用户，umd可以打包输出成各种形式。

library：就是打包输出成的demo名

```js
    output: {
        path: path.join(__dirname + '/dist'),
        filename: 'bundle.js',
        libraryTarget:"umd",
        library:'VueToastExp'
    },
```

***如下几部进行发布***

1. cmd输入 npm adduser , 输入用户名密码邮箱，
2. npm publish ，这一步就已经发布了
3. 补充：：npm help  可以看有哪些语法，npm whoami 可以看当前的用户是谁
4. 记得可以在项目目录下加入README.md文件

每次更新我们的项目的时候，记得cmd去 npm publish重新发布！

### webpack多文件开发

