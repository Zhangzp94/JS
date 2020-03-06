### git教程

安装：官网下载安装即可！

##### 1、git的使用

首次使用需要进行全局配置：

桌面空白处右键 GIt Bash Here

````js
$ git config --global user.name "用户名" //这里用户名写github名，Zhangzp94
$ git config --global user.email "邮箱地址" 
````

a. git初始化

````js
$ mkdir pro_git 
$ cd pro_git
$ git init   //git初始化，会出现一个.git的文件夹！
````

b. Git常用的指令

1）查看当前的状态：git status

2）添加到缓存区：git add 文件名

````js
说明：git add 指令，可以添加一个文件名，也可以添加多个文件名，
语法1：git add 文件名
语法2：git add 文件名 文件名 文件名
语法3：git add . 添加当前的目录
````

3）`$ git commit -m "wrote a readme file"`

用命令`git commit`告诉Git，把文件提交到仓库，-m 后面是对当前文件的说明！

每一次修改文件内容都需要 git add . 和 git commit -m "说明内容"

##### 2、版本回退

查看当前所有的版本 $ git log  或者 $ git log --pretty=oneline

***回退操作***

`$ git reset --hard HEAD^`  HEAD^ 是版本号，就是金黄色的 id

但是：当我们回到过去的以后，你 git log 发现只有过去当前的版本号，再想回到现在的版本号，使用指令

`git reflog` 可以得到最新的commit id ，然后 `git reset --hard HEAD^` 

###### 1）撤销

````
git checkout -- demo.txt
这种指令是你修改文件后，还没有git add，那么此时用这个命令就会恢复到未修改之前！

git reset HEAD demo.txt
这种是你修改了，已经git add了，但是还没有 git commit ，使用命令可以回到工作区！暂存区的内容消失！
然后这时候你再 git checkout -- demo.txt，就回到之前的版本了！

第三种情况：git add + git commit 
这种使用版本回退
````

###### 2）删除文件

````
当创建的文件 git add + git commit 后，需要删除的话！
git rm delete.txt
git commit -m "remove delete.txt" 2个命令就可以直接从版本库删除文件了！


另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
第一步：git reset HEAD test.txt
第二步：git checkout -- test.txt
````



##### 3、常见的操作

***第一种操作 https***

1）先在github上创建一个仓库，拿到仓库的url地址！

然后在本地建立新的文件夹如 shop文件夹，cd到shop，指令：`git clone https://...`   http是github仓库的地址！这时候就会把线上仓库的东西同步到本地。开始是空的！同步后会有一个shop的文件夹名！

2）操作线上仓库github（提交暂存区、提交本地仓库、提交线上仓库、拉去线上仓库）

在本地创建文件夹如 shop， cd 到shop中，在里面创建需要上传的文件如readme.txt，然后

git add 文件名   // git add .
git commit -m "wrote a readme file"    到此的2步就是提交暂存区、提交本地仓库

git push //把自己写的代码提交到线上仓库，注意第一次使用的时候会说没有权限，这时候打开git clone时候产生的.git文件夹，里面有config文件，在里面的url那里加上用户名和密码！最新版本的git会自动提醒你输入的！

git pull //把线上的所有文件同步到本地

3）邀请同事加入开发团队！

想要同事可以修改我的项目可以在github上邀请同事，Settings-->Manage access-->Invite a collaborator

会生成一个连接，然后对方登录自己的github账户，黏贴这个连接进来！这时候同事就可以 git push发布写的东西了！



***第二种操作 ssh***

该方式和前面HTTPS 相比，这是影响github对用户身份的鉴权方式，

生成公私钥文件（要先安装 OpenSSh）然后指令：`ssh-keygen -t rsa -C "你的GitHub邮箱"` 一路回车！此时根据命令内容拿到公钥！将公钥上传到github上，在github上把公钥填写好即可！

步骤：1.生成客户端公私钥文件  2.将公钥上传到github上，在github上把公钥填写好即可

下面的操作：就和https 方式一样，

1.先创建一个要clone的文件夹！

指令：`git clone git@github.com:Zhangzp94/shop.git` 

注意可能会有如下错误：

````js
The authenticity of host 'github.com (13.250.177.223)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

记得写yes 不能直接回车！！！
````

##### 4、分支

````js
查看分支：git branch 
创建分支：git branch 分支名
切换分支：git checkout 分支名  //创建和切换分支可以合为一步：git checkout -b 分支名
删除分支：git branch -d 分支名
合并分支：git merge 被合并的分支名
````

```
分支就是几个人分开开发，最后汇总到主分支上去！一般默认有一个主分支 master。
可以先创建自己的分支 git branch dev ，然后切换进去 git checkout dev ,切换进来后可以修改现在目录中的文件，文件内容也就是master中的内容一样的！修改后，照样 git add 和 git commit -m "..."
这里你切回master分支你会发现，刚才在dev分支下修改的文件内容master中没有生效，这也就是分支的特点！
这时候当你 add commit完后，切到master分支下合并dev子分支! git merge dev ，这时候master分支下的文件就包含了你刚才修改的内容！ git push 到github上！最后删除分支：git branch -d dev ,注意删除dev分支的时候要退出dev分支，比如切回到master分支中！
```

##### 5、冲突的产生和解决

````
下班后，同事有修改了github的文，第二天我没有先git pull，而是修改了部分文件后直接git push，这时候会有冲突！那么这时候可以先git pull，那么文件中就会显示同事和你自己修改的文件内容，然后再发布！
````

````
创建分支dev后，修改了内容然后git add git commit 后，切换到Master分支后，在master分支上修改内容，然后git add git commit ，再合并dev的时候会有冲突！这时候可以手动修改文件，再重新git add git commit，再合并dev分支，就好了！最后删除dev分支！
````



##### 6、图形管理工具

在不喜欢使用指令的情况下，可以使用下面的图形管理工具！

1) Github for Desktop

2) Source tree

3) TortoiseGit

##### 7、忽略文件

场景：在项目目录下有很多万年不变的文件，如css images等文件，或者有我们修改了但是不想上传到远程Github上的文件，这时候我们可以“忽略文件机制”来实现需求。

​	忽略文件需要建立一个名为`.gitignore`的文件，该文件用于声明忽略文件和不忽略文件的规则，规则对该目录及子级目录生效。

​	注意：由于该文件只有后缀名，所有无法直接在windows下直接创建，需要使用Git Bash命令去创建，指令：touch .gitignore 创建！

````
.gitignore文件中--
常见的规则写法如下：
1) /js/   过滤整个文件夹
2) *.zip	过滤所有zip文件
3) /js/do.c  过滤某个具体文件
4) !index.php	不过滤某个具体文件
#开头的都是注释！
````





![logo](C:\project\JavaScript\JS\images\logo.png)







````js
pwd //查看当前在哪个文件夹目录
git init //git初始化，会出现一个.git的文件夹！
git add 文件名 // git add .
git commit -m "wrote a readme file"
git log --pretty=oneline //查看当前的版本号
git reset --hard HEAD^ // 回到之前的版本号
git reflog //回去后，查看所有的版本号，然后再选择要去的版本号 git reset --hard HEAD^
````

