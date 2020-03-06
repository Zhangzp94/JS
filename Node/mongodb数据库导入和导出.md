### mongodb数据库的导入导出

我们在进行数据库的迁移的时候可能会需要进行导入和导出的操作，下面对Mongodb数据库的cmd导入和导出命令做一些解释。

##### 1.导出

***cmd导出数据命令：*** mongoexport -h dbhost -d dbname -c collectionName -o outUrl

-h: mongodb的Ip和端口 如：-h 127.0.0.1:27017（这里写localhost:27017可能会失败）

-d: 你的数据库名字，如 itcase

-c: 数据库itcase下面的集合列表，如 users

-o: 是你需要导出的数据地址，后面可以跟json文件格式，如 C:/mongodbSql/users.json

***下面是打开cmd导出数据***

````
C:\Users\Administrator>mongoexport -h 127.0.0.1:27017 -d itcase -c users -o C:/mongodbSql/users.json 
````

##### 2.导入

***cmd导入数据命令：*** mongoimport -h dbhost -d dbname -c collectionName  inUrl

-h: mongodb的Ip和端口 如：-h 127.0.0.1:27017（这里写localhost:27017可能会失败）

-d: 你的数据库名字，先建立一个数据库名，如 itcase

-c: 数据库itcase下面的集合列表名，这里的名字在cmd输入命令的时候再创建

-o: 是你导入的数据地址，如  C:/mongodbSql/users.json

***下面是打开cmd导出数据***

````
C:\Users\Administrator>mongoimport -h 127.0.0.1:27017 -d itcase -c users C:/mongodbSql/users.json 
````

