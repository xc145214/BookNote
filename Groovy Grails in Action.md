#Grails in Action

===
###1。 入门

1.1 下载与安装
+ 下载安装包
+ 新增GRAILS_HOME 环境变量，指向目录
+ 将bin目录添加到Path 环境变量中

终端窗口输入grails命令行查看结果

1.2 hello world
```
//创建项目
grails create-app helloworld

cd helloworld

//创建一个名为  HelloController.groovy 的控制器
grails create-controller hello

//运行服务器
grails run-app
```
