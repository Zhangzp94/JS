##### 01 vue-cli3查看webpack

查看loader的指令

打开 PowerShell 输入 `npx vue-cli-service inspect --rules`，直接输入vue inspect --rules失败的话是因为是局部安装vue-cli3的，如果是这样的话那肯定用不了vue这个命令的！

##### 02 引入font-awesome 出现错误



<img src="..\images\45.png" alt="45" style="zoom:75%;" />

错误原因是因为我用的styl类型的文件！

解决方法: 参考链接：https://blog.csdn.net/mxf_bear/article/details/80505295

<img src="..\images\46.png" alt="46" style="zoom:75%;" />

