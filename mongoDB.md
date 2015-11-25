#mongoDB

## 安装

+ 启动服务：

```
c:\ENV\MongoDB\Server\3.0\bin>mongod --dbpath "C:\ENV\MongoDB\Server\3.0\data"
```

+ 运行：

```
C:\ENV\MongoDB\Server\3.0\bin>mongo.exe
MongoDB shell version: 3.0.7
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        http://docs.mongodb.org/
Questions? Try the support group
        http://groups.google.com/group/mongodb-user

```

+ 查看

mongodb采用27017端口，那么我们就在浏览器里面键入“http://localhost:27017/”，打开后，mongodb告诉我们在27017上Add 1000可以用http模式查看mongodb的管理信息。


## 操作

+ 导入数据

```
mongoimport --db test --collection restaurants --drop --file primer-dataset.json
```


打开cmd,输入mongo命令打开shell，其实这个shell就是mongodb的客户端，同时也是一个js的编译器，默认连接的是“test”数据库。

+ 创建数据库

如果数据库不存在，则创建数据库，否则切换到指定数据库。
```
use person
```

+ 删除数据库

```
//切换
use person
 
db.dropDatabase()
```

+ insert 

```
db.COLLECTION_NAME.inser(document)

var person = {}
person.name = "liming"
person.age = 20
db.person.insert(person)
```

+ update

```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```
> query : update的查询条件，类似sql update查询内where后面的。
> update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
> upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
> multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
> writeConcern :可选，抛出异常的级别。




