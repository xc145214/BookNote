#Grails in Action

===
###1. 入门

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

//在grails-app/domain/Book.groovy 文件中创建一个域类
grails create-domain-class book

//创建一个名为  HelloController.groovy 的控制器
grails create-controller hello

//运行服务器
grails run-app

//测试
grails test-app

//部署应用程序
grails war

//快速生成一个应用
grails generate-all Book

//跟踪日志
grails -Dgrails.full.stacktrace=true run-app
```
修改指定运行端口：
> 1. 运行指令：`grails -Dserver.port=8090 run-app`

> 2. 在BuildConfig.groovy 中加入：`grails.server.port.http = 9000   //set runtime port`

###2. 配置

2.1 基本配置
Grails提供了一个名为  grails-app/conf/Config.groovy 的文件用来进行一般性配置。
Grails提供了下列配置选项：
+ grails.config.locations - 资源（properties）文件或需要被合并到主配置文件中的附加Grails配置文件的位置
+ grails.enable.native2ascii - 如果你不需要对Grails的i18n资源（properties）文件进行native2ascii的转换，那么就将该选项设为false
+ grails.views.default.codec - 用于设置GSP文件的默认编码体制——可以设置“none”、“html”或“base64”中的一个（默认值为：“none”）。为了降低XSS攻击的风险可以将改选项设为“html”。
+ grails.views.gsp.encoding - 用于GSP源代码文件的文件编码（默认为“utf-8”）
+ grails.mime.file.extensions - 是否使用文件扩展名来表示内容协商中的MIME类型
+ grails.mime.types - 被支持的用于内容协商中的MIME类型对应表
+ grails.serverURL - 一个用于描述绝对链接中服务器URL部分的字符串，其中包括了服务器名称。例如：grails.serverURL="http://my.yourportal.com"。
生成War文件
+ grails.war.destFile - 用来设置 war 命令将把生成的WAR文件放置在什么位置
+ grails.war.dependencies - 一个包含了Ant构建器语法或JAR文件列表的闭包。允许你指定哪些库文件需要被包含在WAR文件中。
+ grails.war.java5.dependencies - 一个JAR文件列表，这些JAR文件是需要被包含在用于JDK 1.5或以上版本的WAR文件里的。
+ grails.war.copyToWebApp - 一个包含了Ant构建器语法的闭包，这些语法应该符合Ant的拷贝语法，例如“fileset()”。该功能允许你控制将“web-app”目录中的哪些内容包含到WAR文件中。
+ grails.war.resources - 一个包含了Ant构建器语法的闭包。允许应用程序在正式生成WAR文件前做一些必要的事情。

2.2 日志
日志基础
Grails使用它的通用配置方式来配置潜在的 Log4j 日志系统。要配置日志你需要修改位于 grails-app/conf 目录下的 Config.groovy 文件。
这个独特的 Config.groovy 文件允许你为开发（development）、测试（test）和生产（production）环境（environments）分别进行日志的配置。Grails将适当地处理  Config.groovy 文件并配置Log4j。
从1.1版本的Grails开始，提供了一个 Log4j DSL，你可以像如下例子一样来配置Log4j：
``` 
log4j = {
    error  'org.codehaus.groovy.grails.web.servlet',  //  controllers
               'org.codehaus.groovy.grails.web.pages' //  GSP
    warn   'org.mortbay.log'
}
```
实际上，每个方法都可以转化为一个日志级别，你可以把你想要记录日志的包名作为方法的参数。以下是一些有用的日志记录器：
org.codehaus.groovy.grails.commons - 记录核心工件的信息，如类加载等。
org.codehaus.groovy.grails.web - 记录Grails的Web请求处理
org.codehaus.groovy.grails.web.mapping - URL映射的调试
org.codehaus.groovy.grails.plugins - 记录插件活动情况
org.springframework -查看Spring在做什么
org.hibernate - 查看Hibernate在做什么
 
顶级日志记录器
顶级日志记录器会被所有其他日志记录器继承。你可以使用root方法来配置顶级日志记录器：
``` 
root {
    error()
    additivity = true
}
```
下边的例子用来配置顶级日志记录器去记录错误级别的信息，它的上方是默认的标准输出目标。你也可以将顶级日志记录器配置为将日志输出到多个已命名的输出目标：
``` 
appenders {
        file name:'file', file:'/var/logs/mylog.log'
}
root {
    debug 'stdout', 'file'
    additivity = true
}
```
这里的顶级日志记录器将日志记录到了两个输出目标——默认的“stdout”输出目标和一个“file”输出目标。
你也可以通过参数方式进入Lorg4J闭包的方式来配置顶级日志记录器：
```
log4j = { root ->
    root.level = org.apache.log4j.Level.DEBUG
    …
}
```
闭包参数“root”是 org.apache.log4j.Logger 的一个实例，因此你可以查阅Log4J的API文档，找出哪些属性和方法对你有用。
 
自定义输出目标
使用Log4j你可以明确的定义输出目标。下边是默认可用的输出目标：
jdbc - 用于将日志输出到JDBC连接的输出目标
console - 用于将日志输出到标准输出的输出目标
file - 用于将日志输出到文件的输出目标
rollingFile - 用于将日志输出到滚动文件集的输出目标
例如你可以配置一个滚动文件输出目标：
``` 
log4j = {
        appenders {
                rollingFile name:"myAppender", maxFileSize:1024, fileName:"/tmp/logs/myApp.log"
        }
}
```
每个进入输出目标的参数都会对应到 Appender 类的一个属性。上边的例子设置了RollingFileAppender 类的name、maxFileSize和fileName属性。
如果你愿意通过自己编程来创建输出目标或者你已经有自己的输出目标实现，那么你可以简单地调用  appender 方法以及输出目标实例：
 ```
import org.apache.log4j.*
log4j = {
        appenders {
                appender new RollingFileAppender(name:"myAppender", maxFileSize:1024, fileName:"/tmp/logs/myApp.log")
        }
}
```
现在你可以将输出目标的名称作为一个唯一值设置到某个日志级别方法中，这样日志就记录到一个特定的输出目标中。这些在上一节讲述过：
 ```
error myAppender:"org.codehaus.groovy.grails.commons"
 ```
 
自定义布局
Log4j DSL默认假设你想要使用 样板布局（PatternLayout） 日志格式。也有如下其他布局可用使用：
xml - 创建一个XML布局日志文件
html - 创建一个HTML布局日志文件
simple - 创建一个简单的纯文本布局日志文件
pattern - 创建一个样板布局日志文件
你可以使用layout设置来指定自定义的样板作为一个输出目标：
``` 
log4j = {
        appenders {
        console name:'customAppender', layout:pattern(conversionPattern: '%c{2} %m%n')
    }
}
```
这样的设置也可以用于内置的“stdout”输出目标，这样会将日志输出到控制台中：
```
log4j = {
    appenders {
        console name:'stdout', layout:pattern(conversionPattern: '%c{2} %m%n')
    }
}
```
完整的堆栈日志跟踪
当发生异常时，会产生大量来自Java和Groovy内部的堆栈日志信息。Grails过滤了那些典型的无关信息，同时聚焦到非Grails/Groovy核心类的信息上。
当这种情况发生时，完整的追踪信息总是会写到 StackTrace 日志记录器。这些日志被记录到一个称为stacktrace.log的文件中 - 当然你也可以修改  Config.groovy 文件来进行你想要的设置。例如，如果你更喜欢将完整的堆栈记录信息输出到标准输出，可以添加这样一行：
 
error stdout:"StackTrace"
你也可以将 grails.full.stacktrace 虚拟机属性设置为  true 来完全禁用堆栈跟踪过滤器：
``` 
grails -Dgrails.full.stacktrace=true run-app
```
约定的日志记录方式
所有的应用程序工件都有一个动态添加的 log 属性。这些工件类型包括 domain类、控制器和标记库等。下边是一个使用例子：
``` 
def foo = "bar"
log.debug "The value of foo is $foo"
```
Grails使用 grails.app.<工件类型>.ClassName 来作为日志记录器的命名。下边是一个如何配置日志记录器去记录不同Grails工件的日志的例子：
 ```
log4j = {
        // 为所有的应用程序工件设置
        info "grails.app"
        // 为一个特定的控制器设置
        debug "grails.app.controller.YourController"
        // 为一个特定的domain类设置
        debug "grails.app.domain.Book"
        // 为所有的标记库设置
        info "grails.app.tagLib"
}
```
工件名称（<工件类型>）也是按照约定命名的，一些常见的如下列表：
bootstrap - 用于系统启动类
dataSource - 用于数据源
tagLib - 用于标记库
service - 用于服务类
controller - 用于控制器
domain - 用于domain实体
 
2.2 多环境配置
Grails支持“多环境配置”的概念。grails-app/conf中的Config.groovy和DataSource.groovy两个文件能够使用ConfigSlurper提供的语法来应用“多环境配置”的特性。以下例子是Grails提供的默认  DataSource 里的定义：
```
dataSource {
    pooled = false                          
    driverClassName = "org.hsqldb.jdbcDriver"       
    username = "sa"
    password = ""                           
}
environments {
    development {
        dataSource {
            dbCreate = "create-drop" // 可选“create”、“createeate-drop”和“update”中的一个
            url = "jdbc:hsqldb:mem:devDB"
        }
    }   
    test {
        dataSource {
            dbCreate = "update"
            url = "jdbc:hsqldb:mem:testDb"
        }
    }   
    production {
        dataSource {
            dbCreate = "update"
            url = "jdbc:hsqldb:file:prodDb;shutdown=true"
        }
    }
}
```
注意配置文件的开头部分提供的是公共配置，紧接着的 environments 代码块则指定了用于独立环境配置的数据源信息，包括dbCreate和url属性。这样的语法也可以用与Config.groovy文件。

2.3 数据源
安装数据源需要JDBC驱动，例如使用MySQL数据库，就需要Connector/J这个JDBC驱动。通常这些JDBC驱动都是以JAR文件格式发行的。将需要的JAR文件放到项目的  lib 目录下即可。
还需在grails-app/conf/DataSource.groovy 的Grails数据库描述文件。这个文件包含了数据源的定义，其中有下列这些设置：
+ driverClassName - JDBC驱动的类名
+ username - 获得JDBC连接需要使用的用户名
+ password - 获得JDBC连接需要使用的密码
+ url - 数据库的JDBC URL
+ dbCreate - 是否从domain模型自动生成数据库
+ pooled - 是否使用连接池（默认为true）
+ logSql - 是否启动SQL日志记录
+ dialect - 用于表示与数据库通讯时应该使用的Hibernate方言的字符串或类。查看 org.hibernate.dialect 一文以便获得可用的方言。
一个用于MySQL数据库的典型配置可以像这样：
 
dataSource {
        pooled = true
        dbCreate = "update"
        url = "jdbc:mysql://localhost/yourDB"
        driverClassName = "com.mysql.jdbc.Driver"
        username = "yourUser"
        password = "yourPassword"   
}

2.3.1 数据源和环境
Grails的数据源定义是“环境感知”的，因此你可以针对需要的环境这样配置：
``` 
dataSource {
        // 这里放置公共设置
}                     
environments {
  production {
     dataSource {
          url = "jdbc:mysql://liveip.com/liveDb"                                    
     }                     
  }
}
```

2.3.2 JNDI 数据源
有时你可能需要通过JNDI去查找一个  数据源。
Grails支持像下边这样的JNDI数据源定义：
```
dataSource {
    jndiName = "java:comp/env/myDataSource"
}
```
JNDI的名称格式在不同的容器中会有不同，但是在定义 数据源 的方式上是一致的。
2.3.3 自定义数据库迁移
DataSource的dbCreate属性是非常重要的，它会指示Grails在运行期间使用GORM类来自动生成数据库表。选项如下：
+ create-drop - 当Grails运行的时候删除并且重新创建数据库。
+ create - 如果数据库不存在则创建数据库，存在则不做任何修改。删除现有的数据。
+ update - 如果数据库不存在则创建数据库，存在则对它进行修改更新。
 
**create-drop 和 create 都会删除所有存在的数据，因此请小心使用！**
在部署 模式下 dbCreate 默认被设置为“create-drop”：
```
dataSource {
        dbCreate = "create-drop" // one of 'create', 'create-drop','update'
}
```
在每次应用程序重启时都会自动删除并重建数据库表。显然，这不应该用于生产环境。
2.4 外部配置
为了支持这种外部配置文件的部署方案，你需要在Config.groovy文件的grails.config.locations设置中指明你的外部配置文件所在位置：
```
grails.config.locations = [ "classpath:${appName}-config.properties",
                            "classpath:${appName}-config.groovy",
                            "file:${userHome}/.grails/${appName}-config.properties",
                            "file:${userHome}/.grails/${appName}-config.groovy"]
```
上边的例子演示了从classpath和USER_HOME这些不同的位置来加载配置文件（包括Java属性（properties）文件和 ConfigSlurper 配置）。
最终所有的配置文件都被合并到了 GrailsApplication 对象的 config 属性中，就可以通过这个属性来获取配置信息了。

####3.ＧＲＯＭ
3.1 domain类可以使用 create-domain-class 命令来创建:
grails create-domain-class Person
这将在 grails-app/domain/Person.groovy 位置上创建类，如下:
```
//添加自定义字段
class Person {   
    //默认添加id
    String name
    Integer age
    Date lastVisit
}
```
在数据库中查看结果：默认添加id 和 version 2个字段
id   version   age   last_visit  name

3.2 CURD
**Create** 
为了创建一个 domain 类，可以使用 Groovy new操作符, 设置它的属性并调用 save:
```
def p = new Person(name:"Fred", age:40, lastVisit:new Date())
p.save()
save 方法将使用底层的Hibernate ORM持久你的类到数据库中。
``` 
**Read**
Grails 会为你的domain类显式的添加一个隐式 id 属性，便于你检索:
``` 
def p = Person.get(1)
assert 1 == p.id
```
get 方法通过你指定的数据库标识符，从db中读取  Person对象。 你同样可以使用 read 方法加载一个只读状态对象:
``` 
def p = Person.read(1)
```
在这种情况下，底层的 Hibernate 引擎不会进行任何脏读检查，对象也不能被持久化。注意,假如你显式的调用 save 方法，对象会回到 read-write 状态.
 
**Update**
更新一个实体, 设置一些属性，然后，只需再次调用 save:
 ```
def p = Person.get(1)
p.name = "Bob"
p.save()
``` 
**Delete**
删除一个实体使用 delete 方法:
 ```
def p = Person.get(1)
p.delete()
 ```
