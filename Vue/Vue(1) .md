

### Vue的基本语法

##### （1）v-cloak v-text v-html的使用

***v-cloak***

- 用于隐藏页面加载时候出现的`{{}}`，给元素加属性`v-cloak`，css属性设置隐藏即可，可以解决闪烁问题。

  ```js
  <style>
          [v-cloak]{display: none}
  </style>
  <!--div这里相当于MVVM中的V，用户界面-->
  <div id="app">
      <p v-cloak>{{ msg }}</p>
      <p v-text="msg2"></p>
      <p v-html="msg3">123</p>
      <botton v-bind:title="msg4">按钮</botton>
  </div>
  ```

***v-text***

- `v-text=" "` 可以设置标签内部的内容，不会有闪烁`{{}}`的问题，但是会覆盖标签内部其他的内容。

***v-html***

- 用于设置HTML文本转化成标签形式的！

***v-bind***

- 用于绑定变量的，具体用法参考上面代码区

- `v-bind:`===`:`

  `<botton v-bind:title="msg4">按钮</botton>`===`<botton :title="msg4">按钮</botton>`

***v-on***

- 用于绑定事件！

  ```js
  <input type="button"value="按钮！" v-on:click="show">
  --
  methods:{
       show:function () {
            alert('hello')
          	}
          }
  ```

- v-on的缩写是`@`

##### （2）事件修饰符

***.stop阻止冒泡***

```js
<div @click="a">
    <input type="text" @click.stop="b">
</div>
```

***.capture捕获事件***

```js
上面.stop是阻止事件冒泡！
而.capture是捕获事件，让事件由外到内触发！
```

***.prevent阻止默认行为***

```js
<a href="" @click.prevent=""></a>
```

***.self***

- 只有点击当前元素的时候才会触发，下面btn不加.stop也可以阻止冒泡！不过只能阻止当前元素的冒泡！

```js
 <div @click.self="b" style="width: 100px;height: 100px;background-color:red;">
        <input type="button" value="btn" @click="a">
    </div>
```

***.once***

```js
只能触发1次事件，比如点击事件，只能点击1次！
```

##### （3）v-model

- `v-model="  "`相比`v-bind:`绑定变量的时候，v-model可以实现双向绑定，只限于表单元素使用！

  就是用户在页面输入数据的时候，后台Js的变量也会跟着一起改变！

- `input select checkbox textarea`

  ```js
   <p>{{ msg }}</p>
      <input type="text" v-model="msg" style="width: 100%">
  ```

##### （4）通过属性为元素绑定类样式

1. 直接给class属性传递一个数组，但是要给class绑定v-bind！数组内部要用`引号`包裹类名，否则就是变量，会按变量的形式到vue的data变量属性里面找！

   ```js
   <h1 v-bind:class="['类名1','类名2',...]">
   ```

2. 在class中使用三元表达式，下面的变量flag是data的属性

   ```js
   <h1 v-bind:class="['color',flag?'active':'']">;//写法一
       
   <h1 v-bind:class="['color',{'active':flag}]">;//写法二，使用对象代替三元表达式
   
   <script>
       var vm = new Vue({
           el:'#app',
           data:{
               flag:true
           },
           methods:{}
       })
   </script>
   ```

3. 直接传递对象的形式，对象的属性是类名，对象的属性带不带引号都可以！

   ```js
   <h1 v-bind:class="{类名1:true,类名2:true}">;//写法一
   
   //写法二
   <h1 v-bind:class="obj">;
   <script>
       var vm = new Vue({
           el:'#app',
           data:{
               obj:{类名1:true,类名2:true}
           },
           methods:{}
       })
   </script>
   
   ```


##### （5）属性绑定元素style内样式

***第一种***

```js
<!--如果属性有 - 横杠，要加引号-->
<div id="app">
    <h3 :style="{color:'red','font-style':'italic','letter-spacing':'20px'}">这是一个H3</h3>
</div>
```

***第二种***

- 通过添加data属性到数组的方式

```js
<div id="app">
    <h3 :style="[obj1,obj2]">这是一个H3</h3>
</div>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            obj1:{color:'red','font-style':'italic'},
            obj2:{'letter-spacing':'20px'}
        },
        methods:{}
    })
</script>
```

##### （6）v-for

***第一：循环数组***

```js
<div id="app">
    <p v-for="(item , index) in arr">值是：{{ item }}--索引是：{{ index }}</p>
</div>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            arr:[11,22,33,44,55],
        }
    })
</script>
```

***第二：循环对象***

```js
<div id="app">
<!--    循环对象，参数1是值 ，参数2是键 ，参数3是索引-->
    <p v-for="(value , key) in arr">值是：{{ value }}--键是：{{ key }}</p>
</div>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            arr:{
                id:1,name:'ck',ip:'上海'
            }
        }
    })
</script>
```

***第三：循环数字***

 ```js
<p v-for="count in 10">值是：{{ count }}</p>;//1-10
 ```

***v-for的key属性的应用：***

- key可以让使用v-for的同时，使得每一项具有唯一性
- key必须用v-bind绑定，key的值必须是Number或者String类型

```js
    <label for="">
        Id:
        <input type="text" v-model="id">
    </label>
    <label for="">
        Name:
        <input type="text" v-model="name">
    </label>
    <input type="button" value="添加" @click="push">
    <p v-for="item in arr" :key="item.id">
        <input type="checkbox">
        Id:{{ item.id }} <==> Name:{{ item.ip }}</p>
</div>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            id:'',
            name:'',
            arr:[
                {id:1,ip:'上海'},
                {id:2,ip:'深圳'},
                {id:3,ip:'北京'},
                {id:4,ip:'南通'}
            ]
        },
        methods:{
            push(){
                this.arr.unshift({id:this.id,ip:this.name})
            }
        }

    })
</script>
```

##### （7）v-if v-show

- 都是隐藏和显示元素的属性

- v-if 是删除和创建元素；v-show是`display:none`去渲染元素的

```js
<input type="button" @click="flag=!flag" value="toggle">
    <h3 v-if="flag">ififif</h3>
    <h3 v-show="flag">showshow</h3>
```

##### （8）安装vuedevtools

- 监听data数据变化的

- 方法：先把chrome文件夹，复制到c盘 C:\Develop文件夹中！打开谷歌浏览器设置--->扩展程序--》勾选开发者模式---》加载已解压的扩展程序---》选择“chrome扩展”文件夹，至此恭喜已经安装成功！！！

  

##### （9）filter方法

- forEach some find filter 方法都是一些循环的方法，some可以通过`return true`终止循环，filter是过滤的，把符合条件的组成新的数组。

- `.includes(字符串)`方法

  ```js
  var newList = arr.filter(item=>{
      if(arr.user.includes('..')){return item};//返回这个项
  })
  newList 是一个新的数组！
  ```

##### （10）过滤器

- 过滤器是用来处理文本内容的

- 定义：`Vue.filter('过滤器名字',function(str){})`

  ```js
  参数1 str 是要过滤的字符串，固定的参数！参数2、3随意传递，
  同时可以调用多个过滤器！
  ```
  
- 私有过滤器

```js
    var vm = new Vue({
        el:'#app',
        data:{
           msg:'这是一首好听的歌，好听的一首歌！'
        },
        methods:{},
        filters:{//私有过滤器，name是过滤器名称
            name:function (msg,n) {
                return  msg.replace(/好听/g,n)
            }
        }
    })
```

- exp:

```js
<!--如果属性有 - 横杠，要加引号-->
<div id="app">
<!--    <h3 :style="[obj1,obj2]">这是一个H3</h3>-->
    <p>{{ msg | name('hi') | name2}}</p>
</div>
<script>
    //过滤器,蚕食1
    Vue.filter('name',function (msg,n) {
      return  msg.replace(/好听/g,n)
    });
    Vue.filter('name2',function (msg) {
        return  msg+'========>>'
    });
    var vm = new Vue({
        el:'#app',
        data:{
           msg:'这是一首好听的歌，好听的一首歌！'
        },
        methods:{},
        filters:{//私有过滤器
            name:function (msg,n) {
                return  msg.replace(/好听/g,n)
            }
        }
    })
</script>
```



##### （11）按键修饰符

```js
<input type="button" @keyup.enter="..." >
@keyup.键盘码="...";//keyCode

方法二：自定义键盘码修饰符
Vue.config.keyCode.f2=113;
<input type="button" @keyup.f2="..." >
```

##### （12）指令函数

***全局写法***

```js
<div id="app">
    //这里的blue 注意是字符串 ！不加字符串就是变量了！
    //注意：如果下面v-color="'blue'"加了blue 不是直接写的v-color那么bind函数中就不能定义死！
    <input type="text" v-focus v-color="'blue'"  v-fontweight="800">

</div>
    Vue.directive('color',{ //directive 指令
       bind:function (el,binding) {//一绑定就会执行！类似于beforeMonted
           //el.style.color='red';
             el.style.color=binding.value;
           //binding.expression 绑定的字符串 表达式
           //binding.value 绑定的字符串值
            console.log(binding.name,'==>',binding.value,'==>',binding.expression)
       },
        inserted:function (el) {
            //把元素插入到父节点中，生命周期，只执行一次！只要调用就插入！
            el.focus()
        },
        updated:function (el,binding) {
			//指令绑定的状态更新的时候执行！
            el.style.color=binding.value;
        }	
    });
```

```js
简写方式！（当插入和更新的逻辑差不多就可以简写，简写是把二者合并）
Vue.directive('color', function(){
    
    
});
```

***用指令初始化swiper***



***私有写法***

```js
    var vm = new Vue({
        el:'#app',
        data:{},
        methods:{},
        filters:{

        },
        directives:{
            //下面这种写法是同时把指令写入到bind update函数中
            'fontweight':function (el,binding) {
                el.style.fontWeight=binding.value;
            },
			'fontsize':{//这种就是一 一的写
                bind:function () {

                }
            },
        },
    })
```

##### （13）实例的生命周期

- 什么是生命周期：从Vue实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期。

- 生命周期钩子：就是生命周期事件的别名。生命周期钩子=生命周期函数=生命周期事件。

- 主要的生命周期函数分类：

  - `beforeCreate`钩子函数

    初始化实例的时候使用，还没有生成data。



##### （14）vue发送ajax请求

- 首先引入`vue-resource`包，

***1.get***

```
methods:{
	this.$http.
}
```

***2.post***

```
参数1：url
参数2：要提交的数据
参数3：配置对象，要以哪种形式提交数据，因为post提交都是表单的提交形式
this.$http.post(url,{},{emulateJSON:true});//emulateJSON:true是vue-resource包提供的，原生是application/x-www-form-urlencoded
```

##### （15）全局配置数据接口的根域名

- 可以把一些请求域名统一化处理

- npm搜`vue-resource` 或 `<https://github.com/pagekit/vueresource/blob/HEAD/docs/config.md>`

  ```
  例子：请求完整的接口地址 http://vue.studyit.io/api/getprodlist
  Vue.http.options.root = 'http://vue.studyit.io/';
  this.$http.get('getprodlist').then(result=>{
                      console.log(result)
  })
  ```

***配置post请求的 emulateJSON:true***

```
Vue.http.options.emulateJSON = true;
```

##### （16）Vue-动画

***1.过渡类名的使用***

- 使用vue提供的元素标签包裹要过渡的
- 定义两组样式来控制 transition 内部的元素及动画

```
.v-enter 这是一个时间点，是动画进入之前
.v-leave-to 这2个是时间点，是动画结束之后
.v-enter-active 入场动画的时间段，
.v-leave-active 离场动画的时间段，

<style>
        .v-enter,.v-leave-to{
            opacity: 0.3;
            transform: translateX(150px);
        }
        .v-enter-active,.v-leave-active{
            transition: all 1s ease ;
        }
</style>

<input type="button" value="点击" @click="flag=!flag">
<transition>
<div>//多个元素过渡
    <h3 v-if="flag" mode="out-in">这是一个h3</h3>
    <div v-else></div>
</div>
</transition>
```

***2.自定义v-前缀***

````
//给transition标签加name属性
<style>
        .my-enter,.my-leave-to{
            opacity: 0.3;
            transform: translateX(150px);
        }
        .my-enter-active,.my-leave-active{
            transition: all 1s ease ;
        }
</style>

<transition name='my'>
    <h3 v-if="flag">这是一个h3</h3>
</transition>
````

***3.transition-group列表动画***

- 列表类动画的时候使用`transition-group`包裹

  ```js
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="vue-2.4.0.js"></script>
      <style>
          li{
              list-style: none;
              margin: 10px 0;
              width: 100% ;
              border: 1px dashed red;
          }
          li:hover{
              background-color:hotpink;
          }
          .v-enter,.v-leave-to{
              opacity: 0.3;
              transform: translate(0,80px);
          }
          .v-enter-active,.v-leave-active{
              transition: all 1s ease;
          }
          /*添加v-move是给删除的li来一个动画，不然li需要等删除后才上移，
              v-move需要配合v-leave-active使用
          */
          .v-move{
              transition: all 1s ease;
          }
          .v-leave-active{
              position: absolute;
          }
  
      </style>
  </head>
  <body>
  <div id="app">
      <label for="">
          Id:
          <input type="text" v-model="id">
      </label>
      <label for="">
          Name:
          <input type="text" v-model="name">
      </label>
      <input type="button" value="添加" @click="add">
  
     <ul>
  <!--appear 属性是添加进场动画效果-->
  <!--tag="ul" 属性是删除系统为Li自动创建的span标签！-->
         <transition-group appear tag="ul">
             <li v-for="(item,i) in list" :key="item.id" @click="del(i)">
                 {{item.id}} --- {{item.name}}
             </li>
         </transition-group>
  
     </ul>
  </div>
  <script>
      var vm = new Vue({
          el:'#app',
          data:{
              id:null,
              name:null,
              list:[
                  {id:1,name:'a'},
                  {id:2,name:'b'},
                  {id:3,name:'c'},
                  {id:4,name:'d'},
              ]
          },
          methods:{
              add(){
                  this.list.push({id:this.id,name:this.name})
                  this.id=parseInt(this.id)+1
                  this.name=''
              },
              del(i){
                  this.list.splice(i,1)
              }
          }
      })
  </script>
  </body>
  </html>
  ```

##### （17）使用animate.css	类库实现动画

例子地址`C:\007\02项目列表\Vue\1219\12过渡动画.html`

可以在github上搜！下翻到 一句话为 `Check out all the animations here!`

### Vue组件

***定义组件***

组件是从UI的角度进行划分！而模块化是根据功能的划分！

***组件的创建方式***

```js
<script>
    //第一种
    //定义组件
   var com = Vue.extend({
       //通过template属性，指定了组件要展示的html结构
        template:'<h3>This is H3</h3>'
    });
   //使用组件Vue.component('组件名',创建出来的组件模板对象)
    //使用Vue.component使用全局组件的时候，如果组件名使用的驼峰命名法，标签需要用'-'链接，把大写转为小写
    Vue.component('mycom',com);
    
    //第二种
    //注意：无论哪种方式创建出来的组件，有且只能有一个根元素，可以用div包裹！
    Vue.component('mycom2',{
        template: '<div><h3>H3H3</h3><span>span</span></div>'
    });

    var vm = new Vue({
        el:'#app',

    })
</script>
    
    //第三种
    Vue.component('mtcom3',{
        template:'#temp';//这里是id，HTML中用 <template id='temp'> ... </template>
    })
    
    var vm = new Vue({
        el:'#app',
        components:{//私有组件
            com:{
                template:'<h1>h1</h1>'
            }
        }

    })
```

***组件中的data***

- 组件中的data需要是function，并且返回的是对象{ }

- 组件中的data数据和实例中的使用方式一样

  ```js
   Vue.component('xxx',{
          template:'<h1>h1h1h1{{ msg }}</h1>',
          data:function () {
              return {
                  msg:'xxxxxx'
              }
          }
      })
  ```

##### （1）组件的切换 component

```js
<div id="app">
<!--    第一种方式v-if v-else切换，缺点：只能切换2个-->
<!--    <input type="button" value="登录" @click="flag=true">-->
<!--    <input type="button" value="注册" @click="flag=false">-->
<!--    <login v-if="flag"></login>-->
<!--    <register v-else="flag"></register>-->

<!--    第二种方式-->
<!--    Vue提供了component标签，通过 :is="com" 绑定 -->
    <input type="button" value="登录" @click="com='login'"> <!--注意这里是字符串-->
    <input type="button" value="注册" @click="com='register'">
        //注意：：切换组件的时候，会删除上一个组件的内容，比如Input的值，所以可以加keep-alive标签！
    <keep-alive>
    	<component :is="com"></component>
	</keep-alive>
</div>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            flag:true,
            com:'login'
        },
        components:{
            login:{
              template:'<h3>登录组件</h3>'
            },
            register:{
                template: '<h3>注册组件</h3>'
            }
        }
    })
</script>
```

***组件切换＋过渡***

```js
       <style>
        .v-enter,.v-leave-to{
            opacity: 0;
            transform: translateX(150px);
        }
        .v-enter-active,.v-leave-active{
            transition: all 1s ease;
        }
    </style>

<transition mode="out-in">
        <component :is="com"></component>
</transition>
```

##### （2）父组件向子级组件传值

```js
<div id="app">
    <!-- 在引用组件的时候，通过属性的绑定 v-bind: 的形式，给子组件绑定一个属性，属性值等于父级组件中的要
    引用的数据  -->
    <com v-bind:parentmsg="msg"></com>
</div>
<script>
    /*
    * 1.子级组件无法访问父级组件中data里面的值!
    * */
    var vm = new Vue({
        el: '#app',
        data: {
            msg: 'abc'
        },
        components: {
            com: {
                data() {
                    return {}
                },
                template: '<h3>登录{{parentmsg}}组件</h3>',
                //子组件中所有 props 的数据，都是父级组件传入的
                //props中的数据都是只读，不可以赋值，父级组件传过来的属性要先在props中定义才行
                props: ['parentmsg']
            },

        }
    })
</script>
```

##### （3）子组件向父组件传值

- 本质就是父组件先向子组件传入一个方法，然后子组件调用方法，并且传入参数，这时候把参数值传入父组件

```js

<div id="app">
<!-- 父组件向子级组件传入方法，用 v-on 事件绑定机制，自定义一个属性之后， -->
    <com v-on:func="show"></com>

</div>

    <template id="tmp">
        <div>
            <h3>子组件</h3>
            <input type="button" value="点击触发父组件传递来得方法func" @click="myclick">
        </div>
    </template>

<script>
  
    var vm = new Vue({
        el: '#app',
        data: {
            msg: null
        },
        methods:{
            show(data){
                console.log(data)
                this.msg=data
            }
        },
        components: {
            com: {
                template:'#tmp',
                data(){
                    return {//这里就是子组件向父组件传入的值
                        someMsg:{id:1,name:'ak'}
                    }
                },
                methods: {
                    myclick(){
                        //点击的时候通过 this.$emit()调用func方法！emit:触发
                        // this.$emit('func'，参数1，参数2)
                        this.$emit('func',this.someMsg)
                    }

                }

            },

        }
    })
</script>
```

##### （4）使用ref获取DOM以及组件，组件通信

```js
<div id="app">
    <input type="text" ref="put">
    <input type="button" value="点击" @click="myClick">
    <com ref="tem"></com>
</div>
<template id="temp" >
<div>
    <span>sjkdnsn</span>
</div>
</template>
<script>
    var vm =new Vue({
        el:'#app',
        data:{

        },
        methods:{
            myClick(){
                console.log(this.$refs.put);
                console.log(this.$refs.tem.my())
            }
        },
        components:{
           com:{
               template:'#temp',
               methods: {
                   my(){
                       console.log('子组件方法！')
                   }
               }
           }
        }

    })

```

##### （5）bus事件总线

- 本质就是几个同级组件之间相互通信关联！

  ```js
  <div id="app">
      <child1></child1>
      <child2></child2>
  </div>
  <script>
      /*
      * 1.要形成几个组件之前的联系，先创建个vue对象
      * 2.在A组件中 bus.$emit('ff',参数1)参数1是赋值对象
      * 3.在B组件中的生命函数中  bus.$on('ff') $on是监听
      * */
      var bus =new Vue();
  
      Vue.component('child1',{
          template:`
              <div>
                  child1
                  <button @click="put">Click</button>
              </div>
          `,
          methods: {
              put() {
                  bus.$emit('ff','child1的值！')
              }
          }
  
      });
      Vue.component('child2',{
          template:`
              <div>
                  child2
                  <p v-show="flag">ppp</p>
              </div>
          `,
          data(){
              return {
                  flag:false
              }
          },
          created(){ 放在生命周期函数中！！！
              bus.$on('ff',res=>{
                  console.log(res)
                  this.flag=!this.flag
              })
          },
          mounted(){
              // bus.$on('ff',res=>{
              //     console.log(res)
              //     this.flag=!this.flag
              // })
          }
  
      });
      var vm =new Vue({
          el:'#app',
          data:{},
          methods:{},
          components:{},
  
      })
  </script>
  ```

##### （6）slot插槽

​		为了组件的可复用性，比如封装了一个轮播图组件，每次调用的时候图片数量不一致，如果你把所有的`li`

一次性写死了，那么调用者调用的时候没法用，因为组件标签内部是不可以自行添加`li`或者其他内容的，会被覆盖删除，此时可以用`slot`定义在组件模板内部，留一个插槽！那么组件标签内部就可以插入内容了！

​		拓展1：当子级组件需要访问父组件的方法的时候，不可以直接访问，需要借助pros属性！这时就可以把子组件的标签拿出来放到父组件作用域中，借助`slot`插槽，就可以直接访问父组件的东西！

​		拓展2：不加name，下面的abc三部分，都会放在组件的三个div中，给插槽加上name，就可以把a放在第一个div中，以此类推！

```js
<div id="app">
    <one>
        <button @click="flag=!flag">click</button>
    </one>
    <two v-show="flag"></two>
    <hello>
        <p slot="a">aaa</p>
        <p slot="b">bbb</p>
        <p slot="c">ccc</p>
    </hello>
</div>
<script>
    Vue.component('one',{
        template:`
          <div>
            <slot></slot>
          </div>
        `,
    });
    Vue.component('two',{
        template:`
          <div>
            <p>ppppppp</p>
          </div>
        `,
    });
    Vue.component('hello',{
        template:`
          <div>
            <div style="background-color: red;"><slot name="a"></slot></div>
            <div><slot name="b"></slot></div>
            <div style="background-color: blue;"><slot name="c"></slot></div>
          </div>
        `,
    });
    var vm =new Vue({
        el:'#app',
        data:{
            flag:true
        },
        methods:{},
        components:{},

    })
</script>
```

##### （7）封装Swiper组件

1. Swiper网址`<https://www.swiper.com.cn/>`

2. 注意：封装swiper组件的时候，注意swiper new的初始化过早的情况，过早的话轮播图不好用！下面会介绍初始化应该放在组件的哪个生命周期函数中！

   ```js
   <div id="app">
   <!--:key="list.length"  是由于初始长度为0，swiper会初始化，有了数据后再初始化！解决初始过早情况-->
       <swiper :key="list.length">
           <div class="swiper-slide" v-for="item in list" >
               {{item}}
           </div>
   
       </swiper>
   </div>
   <script>
       Vue.component('swiper', {
           template: `
       <div class="swiper-container my">
           <div class="swiper-wrapper">
               <slot></slot>
           </div>
   
           <div class="swiper-pagination"></div>
   
       </div>
          `,
           mounted(){
               console.log('mounted')
               new Swiper ('.my', {
                   loop: true, // 循环模式选项
                   // 如果需要分页器
                   pagination: {
                       el: '.swiper-pagination',
                   },
               })
           }
       });
       var vm = new Vue({
           el: '#app',
           data: {
               list:[]
           },
           methods: {},
           components: {},
           mounted(){
               this.list=axios({
                   //拿轮播数据！
               })
           }
       })
   
   
   </script>
   ```
   






###### ***小知识点：***

***第一***

`var dt =new Date(dataStr)`直接new Date()得到是当前的时间，传入一个时间参数，得到是当前的时间，就可以使用一些date方法了！

```js
dt.getFullYear()
dt.getMonth()+1;//从0开始的
dt.getDate();//天
```

***第二***

`String.prototype.padStart(maxlength,'')`

`String.prototype.endStart(maxlength,'')`

上面是字符串的方法，用来填充字符串的，maxlength是填充后字符串的长度，参数2是填充的字符，

```js
dt.getDate().toString().padStart(2,'0');
```

