        vue不涉及 SSR 的时候 直接就是组件渲染，浏览器看不到你组件中的div等一些标签！这样不利于SEO、爬虫，所以下面介绍SSR（服务器渲染）技术。这个就是借助于插件 `vue-server-renderer` 去把原先的组件编译成HTML然后渲染到浏览器中，就可以显示div等标签了。

##### 01 预渲染

只针对部分路由页面进行渲染！比如 / /a /b  ，借助于插件 `prerender-spa-plugin`

https://github.com/chrisvfritz/prerender-spa-plugin

安装：`npm i prerender-spa-plugin -D`

***第一步***

![80](..\images\80.png)

***第二步***

![81](..\images\81.png)





##### 02 服务端渲染

![78](..\images\78.png)