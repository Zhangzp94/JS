# `Vue`（二）

### `Vue`路由

- 前端路由就是通过`hash #号`来实现页面之前的跳转!

##### （1）路由的基本使用

***包含的知识点：***

1.路由的使用

2.`router-link`的使用

3.路由`redirect`的使用

4.为选中的路由设置高亮显示的2种方法

5.为路由切换设置动画效果

6.使用`query传递url参数`

7.使用`params传递url参数`

```js
    <script src="../lib/vue-2.4.0.js"></script>
    <script src="../lib/vue-router-3.0.1.js"></script>
    <style>
/*<!-- router-link 会提供一个类，可以用来设置高亮显示 -->*/
        .router-link-active,.myactive {
            color: red;
            font-size: 30px;
        }
        .v-enter,.v-leave-to{
            transform: translateX(100px);
            opacity: 0;
        }
        .v-enter-active,.v-leave-active{
            transition:all 0.5s ease;
        }
    </style>
</head>
<body>
<div id="app">

<!--    <a href="#/login">登录</a>-->
<!--    <a href="#/register">注册</a>-->

<!--    router-link 默认渲染出一个a 标签；也可以加个 tag=''属性修改渲染出来的标签-->
    <router-link to="/login?id=1&name=校长" tag="span">登录</router-link>
    <router-link to="register/10/ak">注册</router-link>

<!--    mode="out-in"  先出去再进来-->
    <transition mode="out-in">
        <router-view></router-view>
    </transition>
</div>
<script>
    var login = {
        //这里的this可以省去
      template:'<h2>登录{{ $route.query.id }}{{ $route.query.name }}组件</h2>',
        created(){
          //query传递参数
            console.log(this.$route.query.id)
        }
    };
    var register = {
        template:'<h2>注册组件{{$route.params.id}}{{$route.params.name}}</h2>',
        created() {
            //params方式传递参数，见{path:'/register/:id/:name',component:register},
            console.log(this.$route.params)
        }
    };
    var routerObj = new VueRouter({
        //routes 表示路由匹配规则，里面有2个属性，
        //属性1：path 是表示监听哪个路由链接地址
        //属性2：component 是如果path匹配到地址，则展示path对应的那个组件
        //注意：component的属性值必须是一个对象！即组件的模板对象
        routes:[
            {path:'/',redirect:'login'},//重定向，页面一加载就是跳转！
            {path:'/login',component:login},
            {path:'/register/:id/:name',component:register},
        ],
        linkActiveClass:'myactive',//可以自定义一个类名来设置样式，自定义了后原始的失效！
    });
    var vm =new Vue({
        el:'#app',
        data:{

        },
        methods:{

        },
        components:{

        },
        router:routerObj,//将路由规则对象注册到vm实例上，用来监听url地址变化，然后展示对应的组件！

    })
</script>
</body>
</html>
```

***动态路由的传参***

````
<router-link  :to="{path:'orderConfirm',query:{'address':addd}}">
````



##### （2）路由的嵌套

- 使用`Children属性`

```js
<div id="app">
    <router-link to="/account">account</router-link>
    <router-view></router-view>
</div>
<template id="temp">
    <div>
        <h3>Account组件</h3>
        <router-link to="/account/login">登录</router-link>
        <router-link to="/account/register">注册</router-link>
        <router-view></router-view>
    </div>
</template>
<script>
    var account = {
        template:'#temp'
    };
    var login = {
        template: '<h3>登录</h3>'
    };
    var register = {
        template: '<h3>注册</h3>'
    };
    var router = new VueRouter({
        routes:[
            {path:'/account',component:account,
                children:[
                    //这里不需要加/login，加了'/'是默认以跟目录开始，不加会自动拼接上面的/account
                    {path:'login',component:login},
                    {path:'register',component:register}
                ]
            },

        ]
    });
    var vm =new Vue({
        el:'#app',
        data:{

        },
        methods:{

        },
        components:{

        },
        router

    })
</script>
```

##### （3）使用命名视图实现经典布局

```js
<body>
<!--实现三个路由在同一页面的布局-->
<div id="app">
    <router-view></router-view>
    <div class="flex">
<!--添加一个name属性，根据name属性的名字寻找下面的对应要展示的组件-->
        <router-view name="left"></router-view>
        <router-view name="right" ></router-view>
    </div>

</div>
<script>
    var header = {
        template:'<h3>header部分</h3>'
    };
    var left = {
        template:'<h3 class="left">left部分</h3>'
    };
    var right = {
        template:'<h3 class="right">right部分</h3>'
    };
    var router = new VueRouter({
       routes:[
           {path:'/',components: {
               'default':header,
                   'left':left,
                   'right':right
               }}
       ]
    });
    var vm =new Vue({
        el:'#app',
        data:{},
        methods:{},
        components:{},
        router


    })
</script>
</body>
```

###### （3-1）路由的跳转方式

js中 location.href跳转

***方式1：声明式***   通过 router-link  to=" / "

***方式2：编程式--路径***   `this.$router.push('/detail')`注意这里是`$router`不是$route

拓展: `this.$router 和 this.$route` 的区别！

`this.$route`是获取跳转路由的参数的，比如： `this.$route.params.id` 

***方式3：编程式--名字***

```js
假如路由是有名字的！
{
    path:'detail/:id',
    name:'detail',
    component:detail
}
此时跳转的时候，假如点击按钮
clickHander(id){
     this.$router.push({name:'detail',params:{id:id}})
}
```



###### （3-2）动态路由

```js
{
    path:'/detail/:id',//后面加 :id 传参就是动态路由！
    component:detail
}
此时跳转的时候，假如点击按钮
clickHander(id){
     this.$router.push(`/detail/${id}`)
}
```

###### （3-3）路由的别名

```js
{
    path:'/detail',
    component:detail,
    alias:'/b',//别名，用户访问 /b的时候，实际匹配规则还是匹配的  /detail
}
```

###### （3-4）去掉url中的 # 号

hash模式下，浏览器地址栏都会有 #  ，那么怎么去掉 #呢？我们可以用 history模式！

但是这种模式需要后端配合设置！具体参考`<https://router.vuejs.org/zh/guide/essentials/history-mode.html>`

###### （3-5）全局守卫

​		全局守卫类似于就是一个权限卡关，假如访问一些页面需要登录的状态，那么此时就可以用它，就是如果没有登录直接跳转到登录页面！

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {//这里的next相当于node中的next，如果要继续必须 next()
  // ...
    if(to.path==='/center'){//点击个人中心的时候
        if(user.login()){//自己写的，如果登录了
            next()
        }else{
        	next('/login');//next()内部直接传入路径值！    
        }
    }else{//点击其他路径的时候，该怎么跳怎么跳
        next()
    }
})
```

***局部组件守卫***

```js

```

###### （3-6）activeClass=“ ”设置路由自动样式

```js
<router-link to="/film/nowplay" tag="li" activeClass="active">正在热映</router-link>

activeClass="active" 中 activeClass 是给路由设置一个默认的高亮显示样式！ active 是类样式！
```



##### （4）watch监听

```js
<div id="app">
    <input type="text" v-model="first" @keyup="get"> +
    <input type="text" v-model="second" @keyup="get"> =
    <input type="text" v-model="end" >
</div>
<script>
    var vm =new Vue({
        el:'#app',
        data:{
            first:'',
            second:'',
            end:''
        },
        methods:{
            // get(){
            //     this.end=this.first+this.second
            // }
        },
        watch:{//watch 监听元素的变化，是时刻监听的 ,参数1是新的文本，参数2是旧的的文本
            'first':function (newVal,oldVal) {
                this.end=newVal+this.second;
            },
            second(newVal,oldVal) {
                this.end=this.first+newVal;
            }
        },
```



##### （5）computed计算属性

- 注意一点：计算属性是，计算的值发生变化就自动计算！

```js
<div id="app">
    <input type="text" v-model="first" > +
    <input type="text" v-model="second" > =
    <input type="text" v-model="fullname" >
</div>
<script>
    var vm =new Vue({
        el:'#app',
        data:{
            first:'',
            second:'',
            end:''
        },
        computed:{
            //在computed中，可以定义一些属性，这些属性叫做【计算属性】，计算属性
            //的本质就是一个方法，但是我们使用的时候可以直接调用属性！
            //注意1：引用计算属性的时候不加 （）
            //注意2：只要计算属性内部 function的data数据发生了变化，就会重新计算这个属性的值！
            //注意3：计算属性的值结果会被保存在缓存中，如果计算属性中的值没有变化就不会重新计算，多次调用
            //也是从缓存中获取!
            fullname() {
                return this.first + '--' + this.second
            }
            
        }

    })
```

##### （6）methods,watch,computed区别

`computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算，主要当做属性来使用！主要运用于一些计算数据的时候！

`methods`方法表示一个具体的操作，主要书写业务逻辑！

`watch`里面是需要观察的表达式，值是回调函数。watch主要是用来监听一些特定数据的变化，可以看做是

computed 和 methods的结合体！

##### （7）nrm

`nrm`提供了几个常用的下载包地址，并且可以在这几个包之间随意切换，但是nrm只是提供的镜像地址，实际下载还是npm下载！就是使用`nrm use npm` or `nrm use taobao`切换地址后，使用`npm i jquery`下载！

- 安装`nrm` :`npm i nrm -g`
- `nrm ls`查看所有可用镜像地址列表
- 切换`nrm use npm`

##### （8）fetch

- fetch是可以看成一个ajax，替代XHR ，但是它的兼容性不好

- 注意：Fetch 请求默认是不带 cooike 的，需要设置 fetch(credentials:'include')

- 语法：

  ```
  fetch('url').then(res=>{console.log(res)})
  
  --post请求方式
  fetch('url',{
  method:'post',
  headers:{"Content-Type":"application/x-www-form-urlencoded"},
  body:"name=kerwin&age=100",
  credentials:'include',
  }
  ).then(res=>{console.log(res)})
  
  --post请求方式2 ，以json方式请求！
  fetch('url',{method:'post',
  headers:{"Content-Type":"application/json"},
  body:JSON.Stringify({
  	name="kerwin",
  	age=100
  })
  }).then(res=>{console.log(res)})
  ```

##### （9）axios

- axios 也是请求数据的一个库，github直接搜`axios`下载库

- ```
  npm install axios
  ```

```js
axios.get('/user?ID=12345',{params:{name:"andy"}})   //get请求是需要加 params,post请求直接传参
  .then(function (response) {
    // handle success
    console.log(response);
  })
  .catch(function (error) {
    // handle error
    console.log(error);
  })
  .finally(function () {
    // always executed
  });


--第二种方法
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
}).then(res=>{console.log(res)});
```

##### （10）mixins

- 作用：可以混入代码，如方法等

````js
<div id="app">
    <input type="button" @click="methodA" value="click">
</div>
<script>
    var obj = {
        methods:{
        methodA(){
            console.log('AAA')
        },
        methodB(){
            console.log('BBB')
        },
        }
    }
    var vm =new Vue({
        el:'#app',
        data:{},
        methods:{
        },
        mixins:[obj],//把外面的方法直接混入进来！
        components:{},
    })
</script>
````

##### （11）快速创建node项目

```js
先cd到要安装的项目下
# 全局安装express
npm i -g express-generator
# 创建项目
express -e demo
# 进入项目 & 安装模块
cd demo;
npm i
```

##### （12）keep-alive标签

````js
keep-alive标签去包裹router-view标签！这样在组件来回切换的过程中，就不会上一个组件的mounted中的请求数据就不会重新渲染一次，而是保存在缓存中！
比如：组件1，请求数据渲染轮播图，当这时候用户切换到组件2的时候，再切换成组件1的时候就重新请求了数据，从轮播图1开始轮了，影响体验！
````



### 脚手架vue.cli

网站：`<https://cli.vuejs.org/zh>`

安装：`npm i  @vue/cli -g`

```js
创建项目： vue create myapp
开发环境构建：npm run serve
生产环境构建：npm run build
代码检测工具（会自动修正）：npm run lint

参考链接：https://www.cnblogs.com/Free-Thinker/p/10725569.html
```



##### （1）配置反向代理解决跨域

​		先建立`vue.config.js`配置文件。

1. proxy代理

   ```js
   --vue.config.js文件中
   module.exports = {
     devServer: {
       proxy: {
         '/api': { 
           target: 'http://maoyan.com',
           //ws: true, //是否自动打开页面
           changeOrigin: true
         },
         '/api/*': {   ///api/*加*号 可以匹配所有！
           target: 'http://maoyan.com',
           //ws: true, //是否自动打开页面
           changeOrigin: true
         },
         '/foo': { 配置多个跨域
           target: '<other_url>'
         }
       }
     }
   }
   
   如：访问 http://maoyan.com/api/abc.html
   axios.get('/api/abc.html').then()
   ```

##### （2）vue多页面开发

​		就是在同一个vue.cli 项目下，有多个入口文件! 需要在 vue.config.js文件中进行配置! 具体参考vue.cli网站的pages中的配置！`<https://cli.vuejs.org/zh/config/#pages>`

##### （3）json-server模拟数据库

​		这个是利用一个json文件搭建一个简单的数据库文件，可以通过接口的形式访问这个文件的数据！

​		github上搜 `json-server`

​		安装：`npm install -g json-server`

​		用法：cmd 直接cd到存放json文件的文件夹中！然后运行`json-server --watch db.json`

### Vuex介绍

***作用：***1. 状态共享，解决非父子组件通信；2. 做数据缓存（如页面数据不是经常需要更新，可以在首次请求的时候把数据缓存起来，避免多次请求后端数据，影响体验），只要页面不刷新，就一直从缓存中拿数据；3. 调式代码 ；

Vuex 是一个专门为Vue.js应用程序开发的状态管理模式，状态管理模式-->就是去集中管理组件中的data部分。

***重点：***Vuex的工作流程是什么？

​				    	--> Dispatch		Actions（异步）	-->  Commit

Vue components																	Mutations

​						Render <--		   State（同步） 	  Mutate <--

从组件中  Dispatch 到 Actions中去做异步处理，异步返回的数据 Commit（提交） 到 Mutations中，通过Mutations  Mutate （改变） 状态，直至影响组件！

***安装：***<https://vuex.vuejs.org/zh/installation.html>

```bash
npm install vuex --save
```

##### 1、同步

```js

--组件文件中
created() {
    this.$store.commit('showchange',true)
}
beforeDestroy() {
      this.$store.commit('hidechange',false)
}

--store.js文件中
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    show:true
  },
  mutations: {
    showchange(state,payload){
      state.show=payload
    },
    hidechange(state,payload){
      state.show=payload
    }
  }
})

组件文件中，commit提交，参数1是 mutations中的函数名，参数2是传递的值！
store.js文件中，mutations 中的处理函数，参数1是 state ，参数2 是上面传递的值！
```

##### 2、异步

下面的案例主要是点击单页面，从缓存中拿数据

```js
created() {
   if(this.$store.state.list.length===0){
      //这里发请求！
      this.$store.dispatch('increment');//1
   }else{
      
   }
}

--store.js文件中
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex);
export default new Vuex.Store({
  state: {
    show:true,
    list:[]
  },
  mutations: {
    mutation(state,payload){
       state.list= payload//3这里最后拿到数据！
    }
  },
  actions:{
      increment(store){ //2  ;store 指的是Vuex对象
           axios({
          	url:"",
      	}).then(res=>{
      	   res.data
           store.commit('mutation',res.data)//放到mutation中执行！
      	})
      } 
  }
})
```

##### 3、计算属性写法

```js
先介绍一下 ... 这个是es6的语法，分割作用！
--组件文件中 
// mapState 可以把 vuex中的state属性内容直接映射到这个组件文件中！
//参考：https://www.jb51.net/article/138460.htm
import {mapState} from 'veux'
computed:mapState(['list'])   //mapState(['list'])=== {list(){return ..}}
就相当于  computed:{ list(){return ..} }	
当组件中还需要写其他计算属性的时候，上面的没有{}，所以不好写，此时可以用...  把mapState(['list'])
里面的内容分割出来，可以理解成 ... 就是把上面对象破开，去掉{}。
----
exp:
var a = [1,2,3]
[...a];//[1,2,3]  ...把a的[]去掉了，不就放到下面的[]中了
---
computed:{
    ...mapState(['list']),//这里===  list(){return ..}
    a(){},
  	
}
```

##### 4、getters

getters 就是对state里面的数据进行处理，比如我只想用state里面list数组的部分内容，就是可以getters中创建一个方法去处理这个数组，那么其他组件就可以调用了！

```js
export default new Vuex.Store({ 
	state:{
        list:[]
    },
    mutations:{},
    actions:{},
    getters:{//如果对state里面的数据不满意，这里可以去处理state的数据，
        filter(state){
            state.list.filter((item,index=>index<5))
        }
    }
})

--组件页面
this.$store.getters.filter;
或者computed写法
import {mapGetters} from 'vuex'
computed:{
    ...mapGetters(['filter'])
}
```

##### 5、mutations

mutations 主要是更改 state中数据的状态的。

```js
--a.js文件
var name =  "showchange"
export {name}
--store.js文件中
import {name} from './a.js'
主要讲mutations的常量写法
export default new Vuex.Store({
	state:{},
	mutations:{
     [name](state,payload){ //这里的 [name]就是 利用常量写法控制的！
      state.show=payload
    	}
    }
})

exp:
var name = "ak"
var obj = {
    [name] : "xx" ;//此时obj里面没有 name 这个属性了！
}
//  obj.ak=xx
```

***State***

State是唯一的数据源，组件data中的数据变量都是放在State中去集中管理。

State是单一的状态数

##### 6、modules

面对复杂的应用程序，当管理的状态较多的时候，我们需要将Vuex的Store对象分割成模块（modules）。

````js
const moduleA = {
    state:{},
    mutations:{},
    actions:{},
    getters:{}
}
const moduleB = {
    state:{},
    mutations:{},
    actions:{},
    getters:{}
}
const store = new Vuex.Store({  //合并上面的A B
    modules:{
        a:moduleA,
        b:moduleB
    }
})
````

 