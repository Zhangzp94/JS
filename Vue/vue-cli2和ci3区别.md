##### 01 vue-cli2文件夹

![47](..\images\47.png)



这里的static文件夹就相当于cli3的public文件夹，cle3没有static文件夹了，打包的时候public文件夹原封不动的打包到dist文件夹中！



##### 02 安装脚手架错误

解决方法1：cmd右键管理员身份输入 npm clean cache -force ，这个目的是删除 C:\Users\Administrator\AppData\Roaming\npm-cache 这个npm-cache文件夹！



##### 03 vue文件讲解

<img src="..\images\48.png" alt="48" style="zoom:80%;" />



##### 04 runtimecompiler和runtimeonly区别

下面讲述了template一直到UI的过程！下面是cli2中的

![50](..\images\50.png)



在安装cli的时候还有这2个选项，runtime 运行时，compiler 编译器，下图是区别



![49](..\images\49.png)



```js
new Vue({
  router,
  store,
  render: h => h(App)
  /*
  * 这里的h就相当于是一个 creatElement('标签')函数，把传入的app创建好去
  * 替换入口文件index.html的id=app的div，就是说渲染好后，之前的哪个id=app的div就没有了！
  * function (creatElement) {
  * 第一种用法：
  *   return creatElement('h2',{class:'box'},['hello world',creatElement('button',['按钮'])])
  * }
  * 第二种用法：
  * 传入组件的方法！
  * function (creatElement){
  *   return creatElement('组件名') 就是  render: h => h(App)
  * }
  * 这里的App组件被传入的时候不是template形式的组件，已经被vue-template-compiler这个模块解析成了对象！
  * 所以就少了一个template-ast的过程！
  * */
}).$mount("#app");

render方法的本质就是把tempalte编译，用creatElement()方法去渲染dom
```



![51](..\images\51.png)



##### 05 vue-cli3的安装目录解释



![52](..\images\52.png)



![53](..\images\53.png)



<img src="..\images\54.png" alt="54" style="zoom:80%;" />



![55](..\images\55.png)



##### 06 cli3的webpack配置去哪儿了？

**方法1**

cmd输入 vue ui 



**方法2**

打开node_modules文件夹

<img src="..\images\56.png" alt="56" style="zoom:80%;" />



**方法3**

直接根目录创建 vue.config.js文件，到时候vue会把这里面的配置和webpack里面的配置合并！



##### 07 引入stylus文件出错？

下图是stylus 文件中的样式 ，url路径要写对！

![57](..\images\57.png)





<img src="..\images\58.png" alt="58" style="zoom:80%;" />



**引入样式的时候，组件的style中，@impot  ~@/...  的格式引入，在js文件中引入样式直接 **

**import './...'**

**引入组件：import ... from ...**

****



##### 08 vue中快速修改端口号



![79](..\images\79.png)