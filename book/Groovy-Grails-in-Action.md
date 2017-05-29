# Grails in Action

[TOC]

## 1. 入门

### 1.1 下载与安装
+ 下载安装包
+ 新增GRAILS_HOME 环境变量，指向目录
+ 将bin目录添加到Path 环境变量中

终端窗口输入grails命令行查看结果

### 1.2 hello world
```groovy
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

## 2. 配置

### 2.1 基本配置
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

### 2.2 日志
日志基础
Grails使用它的通用配置方式来配置潜在的 Log4j 日志系统。要配置日志你需要修改位于 grails-app/conf 目录下的 Config.groovy 文件。
这个独特的 Config.groovy 文件允许你为开发（development）、测试（test）和生产（production）环境（environments）分别进行日志的配置。Grails将适当地处理  Config.groovy 文件并配置Log4j。
从1.1版本的Grails开始，提供了一个 Log4j DSL，你可以像如下例子一样来配置Log4j：
```groovy
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
```groovy
root {
    error()
    additivity = true
}
```
下边的例子用来配置顶级日志记录器去记录错误级别的信息，它的上方是默认的标准输出目标。你也可以将顶级日志记录器配置为将日志输出到多个已命名的输出目标：
```groovy 
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
```groovy
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
``` groovy
log4j = {
        appenders {
                rollingFile name:"myAppender", maxFileSize:1024, fileName:"/tmp/logs/myApp.log"
        }
}
```
每个进入输出目标的参数都会对应到 Appender 类的一个属性。上边的例子设置了RollingFileAppender 类的name、maxFileSize和fileName属性。
如果你愿意通过自己编程来创建输出目标或者你已经有自己的输出目标实现，那么你可以简单地调用  appender 方法以及输出目标实例：
 ```groovy
import org.apache.log4j.*
log4j = {
        appenders {
                appender new RollingFileAppender(name:"myAppender", maxFileSize:1024, fileName:"/tmp/logs/myApp.log")
        }
}
 ```
现在你可以将输出目标的名称作为一个唯一值设置到某个日志级别方法中，这样日志就记录到一个特定的输出目标中。这些在上一节讲述过：
 ```groovy
error myAppender:"org.codehaus.groovy.grails.commons"
 ```

自定义布局
Log4j DSL默认假设你想要使用 样板布局（PatternLayout） 日志格式。也有如下其他布局可用使用：
xml - 创建一个XML布局日志文件
html - 创建一个HTML布局日志文件
simple - 创建一个简单的纯文本布局日志文件
pattern - 创建一个样板布局日志文件
你可以使用layout设置来指定自定义的样板作为一个输出目标：
``` groovy
log4j = {
        appenders {
        console name:'customAppender', layout:pattern(conversionPattern: '%c{2} %m%n')
    }
}
```
这样的设置也可以用于内置的“stdout”输出目标，这样会将日志输出到控制台中：
```groovy
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
``` groovy
grails -Dgrails.full.stacktrace=true run-app
```
约定的日志记录方式
所有的应用程序工件都有一个动态添加的 log 属性。这些工件类型包括 domain类、控制器和标记库等。下边是一个使用例子：
``` groovy
def foo = "bar"
log.debug "The value of foo is $foo"
```
Grails使用 grails.app.<工件类型>.ClassName 来作为日志记录器的命名。下边是一个如何配置日志记录器去记录不同Grails工件的日志的例子：
 ```groovy
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

### 2.3 多环境配置

Grails支持“多环境配置”的概念。grails-app/conf中的Config.groovy和DataSource.groovy两个文件能够使用ConfigSlurper提供的语法来应用“多环境配置”的特性。以下例子是Grails提供的默认  DataSource 里的定义：
```groovy
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

### 2.4 数据源

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
```groovy
dataSource {
        pooled = true
        dbCreate = "update"
        url = "jdbc:mysql://localhost/yourDB"
        driverClassName = "com.mysql.jdbc.Driver"
        username = "yourUser"
        password = "yourPassword"   
}
```
#### 2.4.1 数据源和环境

Grails的数据源定义是“环境感知”的，因此你可以针对需要的环境这样配置：
``` groovy
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

#### 2.4.2 JNDI 数据源

有时你可能需要通过JNDI去查找一个  数据源。
Grails支持像下边这样的JNDI数据源定义：
```groovy
dataSource {
    jndiName = "java:comp/env/myDataSource"
}
```
JNDI的名称格式在不同的容器中会有不同，但是在定义 数据源 的方式上是一致的。

#### 2.4.3 自定义数据库迁移

DataSource的dbCreate属性是非常重要的，它会指示Grails在运行期间使用GORM类来自动生成数据库表。选项如下：
+ create-drop - 当Grails运行的时候删除并且重新创建数据库。
+ create - 如果数据库不存在则创建数据库，存在则不做任何修改。删除现有的数据。
+ update - 如果数据库不存在则创建数据库，存在则对它进行修改更新。

**create-drop 和 create 都会删除所有存在的数据，因此请小心使用！**
在部署 模式下 dbCreate 默认被设置为“create-drop”：
```groovy
dataSource {
        dbCreate = "create-drop" // one of 'create', 'create-drop','update'
}
```
在每次应用程序重启时都会自动删除并重建数据库表。显然，这不应该用于生产环境。

#### 2.4.4 外部配置

为了支持这种外部配置文件的部署方案，你需要在Config.groovy文件的grails.config.locations设置中指明你的外部配置文件所在位置：
```
grails.config.locations = [ "classpath:${appName}-config.properties",
                            "classpath:${appName}-config.groovy",
                            "file:${userHome}/.grails/${appName}-config.properties",
                            "file:${userHome}/.grails/${appName}-config.groovy"]
```
上边的例子演示了从classpath和USER_HOME这些不同的位置来加载配置文件（包括Java属性（properties）文件和 ConfigSlurper 配置）。
最终所有的配置文件都被合并到了 GrailsApplication 对象的 config 属性中，就可以通过这个属性来获取配置信息了。

## 3.GROM

### 3.1 domain类可以使用 create-domain-class 命令来创建:

grails create-domain-class Person
这将在 grails-app/domain/Person.groovy 位置上创建类，如下:

```groovy
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

### 3.2 CURD

**Create** 

为了创建一个 domain 类，可以使用 Groovy new操作符, 设置它的属性并调用 save:
```groovy
def p = new Person(name:"Fred", age:40, lastVisit:new Date())
p.save()
save 方法将使用底层的Hibernate ORM持久你的类到数据库中。
```

**Read**

Grails 会为你的domain类显式的添加一个隐式 id 属性，便于你检索:
``` groovy
def p = Person.get(1)
assert 1 == p.id
```
get 方法通过你指定的数据库标识符，从db中读取  Person对象。 你同样可以使用 read 方法加载一个只读状态对象:
``` groovy
def p = Person.read(1)
```
在这种情况下，底层的 Hibernate 引擎不会进行任何脏读检查，对象也不能被持久化。注意,假如你显式的调用 save 方法，对象会回到 read-write 状态.

**Update**

更新一个实体, 设置一些属性，然后，只需再次调用 save:
 ```groovy
def p = Person.get(1)
p.name = "Bob"
p.save()
 ```

**Delete**

删除一个实体使用 delete 方法:
 ```groovy
def p = Person.get(1)
p.delete()
 ```

### 3.3 **GORM中的关联**

#### 3.3.1 one-to-one

+ 单项关联
```groovy

class Face {
    Nose nose
}
class Nose {       
}
```
+ 双向关联
```groovy
calss Face{
    Nose nose
}
class Nose{
    Face face
}
```
+ 级联更新
```groovy
//belongsTo
class Face {
    Nose nose
}
class Nose {       
        static belongsTo = [face:Face]
}
```
在这种情况下，我们使用 belongsTo 来设置Nose "属于" Face。结果是，我们创建一个Face并save 它，数据库将 级联 更新/插入  Nose:
`new Face(nose:new Nose()).save()`
上面的示例，face 和 nose都会被保存。注意，逆向 不为 true，并会因为一个临时的Face导致一个错误:
`new Nose(face:new Face()).save() // will cause an error`
belongsTo另一个重要的意义在于，假如你删除一个 Face 实体， Nose 也会被删除:
``` groovy
def f = Face.get(1)
f.delete() // both Face and Nose deleted
```
如果没有belongsTo ，deletes 将不被级联，并会得到一个外键约束错误，除非你明确的删除Nose:
```groovy
// error here without belongsTo
def f = Face.get(1)
f.delete()
// no error as we explicitly delete both
def f = Face.get(1)
f.nose.delete()
f.delete()
```
你可以保持上面的关联为单向，为了保证级联保存/更新,可以像下面这样:
 ```groovy
class Face {
    Nose nose
}
class Nose {       
        static belongsTo = Face
}
 ```
注意，在这种情况下，我们没有在belongsTo使用map语法声明和明确命名关联。Grails 会把它当做单向。

#### 3.3.2 one-to-many

one-to-many 关联是，当你的一个类，比如 Author ，拥有许多其他类的实体，比如 Book 。 在Grails 中定义这样的关联可以使用 hasMany :
 ```groovy
class Author {
    static hasMany = [ books : Book ]
    String name
}
class Book {
        String title
}
 ```
在这种情况下，拥有一个单向的one-to-many关联。 Grails 将默认使用一个连接表映射这样的关联。
对于 hasMany 设置，Grails将自动注入一个java.util.Set类型的属性到domain类。用于迭代集合:
 ```groovy
def a = Author.get(1)
a.books.each {
        println it.title
}
 ```
默认的级联行为是级联保存和更新，但不删除，除非 belongsTo 被指定:
 ```groovy
class Author {
    static hasMany = [ books : Book ]
    String name
}
class Book {
        static belongsTo = [author:Author]
        String title
}
 ```
如果在one-to-many的多方拥有2个同类型的属性，必须使用mappedBy 指定哪个集合被映射:
 ```groovy
class Airport {
        static hasMany = [flights:Flight]
        static mappedBy = [flights:"departureAirport"]
}
class Flight {
        Airport departureAirport
        Airport destinationAirport
}
 ```
如果多方拥有多个集合被映射到不同的属性，也是一样的:
 ```groovy
class Airport {
        static hasMany = [outboundFlights:Flight, inboundFlights:Flight]
        static mappedBy = [outboundFlights:"departureAirport", inboundFlights:"destinationAirport"]
}
class Flight {
        Airport departureAirport
        Airport destinationAirport
}
 ```

#### 3.3.3 Many-to-Many

Grails支持many-to-many关联，通过在关联双方定义 hasMany ，并在关联拥有方定义 belongsTo :
 ```groovy
class Book {
   static belongsTo = Author
   static hasMany = [authors:Author]
   String title
}
class Author {
   static hasMany = [books:Book]
   String name
}
 ```
Grails在数据库层使用一个连接表来映射many-to-many，在这种情况下，Author 负责持久化关联，并且是唯一可以级联保存另一端的一方 。
例如，下面这个可以进行正常级联保存工作:
 ```groovy
new Author(name:"Stephen King")
                .addToBooks(new Book(title:"The Stand"))
                .addToBooks(new Book(title:"The Shining"))           
                .save()
 ```
而下面这个只保存 Book而不保存 authors!
``` groovy
new Book(name:"Groovy in Action")
                .addToAuthors(new Author(name:"Dierk Koenig"))
                .addToAuthors(new Author(name:"Guillaume Laforge"))             
                .save()
```
这是所期待的行为，就像Hibernate，只有many-to-many的一方可以负责管理关联。

### 3.4 集合类型

#### 3.4.1  集合基础

除了关联不同 domain 类外, GORM 同样支持映射基本的集合类型。比如，下面的类创建一个 nicknames 关联， 它是一个  String 的 Set 实体:
```groovy
class Person {
    static hasMany = [nicknames:String]
}
```
GORM 将使用一个链接表，来映射上面的关联。你可以使用joinTable参数来改变各式各样的连接表映射:
 ```groovy
class Person {
    static hasMany = [nicknames:String]
    static mapping = {
       hasMany joinTable:[name:'bunch_o_nicknames', key:'person_id', column:'nickname', type:"text"]       
    } 
}
 ```
上面的示例映射到表后看上去像这样:

| bunch_o_nicknames | Table    |
| ----------------- | -------- |
| person_id         | nickname |
| 1                 | Fred     |

#### 3.4.2 Set

默认情况下，在中 GORM定义一个 java.util.Set 映射，它是无序集合，不能包含重复元素。 换句话，当你有:
```groovy
class Author {
   static hasMany = [books:Book]
}
```
GORM会将books注入为  java.util.Set类型。问题在于存取时，这个集合的无序的，可能不是你想要的。为了定制序列，你可以设置为 ` SortedSet`:
``` groovy
class Author {
   SortedSet books
   static hasMany = [books:Book]
}
```
在这种情况下，需要实现` java.util.SortedSet` ，这意味着，你的Book类必须实现 `java.lang.Comparable`:
 ```groovy
class Book implements Comparable {
   String title
   Date releaseDate = new Date()
   int compareTo(obj) {
       releaseDate.compareTo(obj.releaseDate)
   }
}
 ```
上面的结果是，Author类的中的books集合将按Book的releasedate排序。

#### 3.4.3 List

如果你只是想保持对象的顺序，添加它们和引用它们通过索引，就像array一样，你可以定义你的集合类型为 List:
 ```groovy
class Author {
   List books
   static hasMany = [books:Book]
}
 ```
在这种情况下当你向books集合中添加一个新元素时,这个顺序将会保存在一个从0开始的列表索引中,因此你可以:
 ```groovy
author.books[0] // get the first book
 ```
这种方法在数据库层的工作原理是:为了在数据库层保存这个顺序,Hibernate创建一个叫做books_idx的列,它保存着该元素在集合中的索引.
当使用List时,元素在保存之前必须先添加到集合中,否则Hibernate会抛出异常 `(org.hibernate.HibernateException: null index column for collection)`:
 ```groovy
// This won't work!
def book = new Book(title: 'The Shining')
book.save()
author.addToBooks(book)
// Do it this way instead.
def book = new Book(title: 'Misery')
author.addToBooks(book)
author.save()
 ```

#### 3.4.4 Map

如果你想要一个简单的 string/value 对map,GROM可以用下面方法来映射:
```groovy
class Author {
   Map books // map of ISBN:book names
}
def a = new Author()
a.books = ["1590597583":"Grails Book"]
a.save()
```
这种情况map的键和值都必须是字符串.
如果你想用一个对象的map,那么你可以这样做:
 ```groovy
class Book {
  Map authors
  static hasMany = [authors:Author]
}
def a = new Author(name:"Stephen King")

def book = new Book()
book.authors = [stephen:a]
book.save()
 ```
static hasMany 属性定义了map中元素的类型,map中的key **必须** 是字符串.

#### 3.4.5 集合类型和性能

Java中的 Set 是一个不能有重复条目的集合类型. 为了确保添加到  Set 关联中的条目是唯一的，Hibernate 首先加载数据库中的全部关联. 如果你在关联中有大量的条目，那么这对性能来说是一个巨大的浪费.
这样做就需要 List 类型, 因为Hibernate需要加载全部关联以维持供应. 因此如果你希望大量的记录关联，那么你可以制作一个双向关联以便连接能在反面被建立。例如思考一下代码:
 ```groovy
def book = new Book(title:"New Grails Book")
def author = Author.get(1)
book.author = author
book.save()
 ```
在这个例子中关联链接被child (Book)创建，因此没有必要手动操作集合以使查询更少和高效代码。由于Author有大量的关联的Book 实例，如果你写入像下面的代码，你可以看到性能的影响：
 ```groovy
def book = new Book(title:"New Grails Book")
def author = Author.get(1)
author.addToBooks(book)
author.save()
 ```

### 3.5 **持久化基础**

Grails的底层使用 Hibernate 来进行持久化.本质上,Grails自动绑定Hibernate session到当前正在执行的请求上.这允许你像使用GORM的其他方法一样很自然地使用 save 和 delete 方法.

#### 3.5.1 save and update

下面看一个使用 save 方法的例子:
 ```groovy
def p = Person.get(1)
p.save()
 ```

一个主要的不同是当你调用save的时候Hibernate不会执行任何SQL操作. Hibernate通常将SQL语句分批,最后执行他们.对你来说,这些一般都是由Grails自动完成的,它管理着你的Hibernate session.
也有一些特殊情况,有时候你可能想自己控制那些语句什么时候被执行,或者用Hibernate的术语来说,就是什么时候session被"flushed".要这样的话,你可以对save方法使用flush参数:
``` groovy
def p = Person.get(1)
p.save(flush:true)
```

请注意，在这种情况下，所有暂存的SQL语句包括以往的保存将同步到数据库。这也可以让您捕捉任何被抛出的异常，这在涉及乐观锁高度并发的情况下是很常用的：
``` groovy
def p = Person.get(1)
try {
        p.save(flush:true)
}
catch(Exception e) {
        // deal with exception
}
```

#### 3.5.2 delete

下面是 delete 方法的一个例子:
 ```groovy
def p = Person.get(1)
p.delete()
 ```

默认情况下在执行delete以后Grails将使用事务写入, 如果你想在适当的时候删除，这时你可以使用flush 参数:
``` groovy
def p = Person.get(1)
p.delete(flush:true)
```

使用 flush 参数也允许您捕获在delete执行过程中抛出的任何异常. 一个普遍的错误就是违犯数据库的约束, 尽管这通常归结为一个编程或配置错误. 下面的例子显示了当您违犯了数据库约束时如何捕捉DataIntegrityViolationException:
``` groovy
def p = Person.get(1)
try {
        p.delete(flush:true)
}
catch(org.springframework.dao.DataIntegrityViolationException e) {
        flash.message = "Could not delete person ${p.name}"
        redirect(action:"show", id:p.id)
}
```

注意Grails没有提供 deleteAll 方法,因为删除数据是discouraged的，而且通常可以通过布尔标记/逻辑来避免.
如果你确实需要批量删除数据,你可以使用 executeUpdate 法来执行批量的DML语句:
``` groovy
Customer.executeUpdate("delete Customer c where c.name = :oldName", [oldName:"Fred"])
```

#### 3.5.3 级联更新和删除

在使用GORM时,理解如何级联更新和删除是很重要的.需要记住的关键是 belongsTo 的设置控制着哪个类"拥有"这个关联.
无论是一对一,一对多还是多对多,如果你定义了 belongsTo ,更新和删除将会从拥有类到被它拥有的类(关联的另一方)级联操作.
如果你 没有 定义 belongsTo 那么就不能级联操作,你将不得不手动保存每个对象.
下面是一个例子:
``` groovy
class Airport {
        String name
        static hasMany = [flights:Flight]
}
class Flight {
        String number
        static belongsTo = [airport:Airport]
}
```

如果我现在创建一个 Airport 对象,并向它添加一些 Flight 它可以保存这个 Airport 并级联保存每个flight,因此会保存整个对象图:
``` groovy
new Airport(name:"Gatwick")
         .addToFlights(new Flight(number:"BA3430"))
         .addToFlights(new Flight(number:"EZ0938"))
         .save()
```

相反的,如果稍后我删除了这个 Airport 所有跟它关联的  Flight也都将会被删除:
``` groovy
def airport = Airport.findByName("Gatwick")
airport.delete()
```

然而,如果我将 belongsTo 去掉的话,上面的级联删除代码就了. 不能工作. 为了更好地理解, take a look at the summaries below that describe the default behaviour of GORM with regards to specific associations.

+ 设置了belongsTo的双向一对多

``` groovy
class A { static hasMany = [bees:B] }
class B { static belongsTo = [a:A] }
```

如果是双向一对多，在多的一端设置了belongsTo，那么级联策略将设置一的一端为"ALL"，多的一端为"NONE".

+ 单向一对多

``` groovy
class A { static hasMany = [bees:B] }
class B {  }
```

如果是在多的一端没有设置belongsTo单向一对多关联，那么级联策略设置将为"SAVE-UPDATE".

+ 没有设置belongsTo的双向一对多

```groovy
class A { static hasMany = [bees:B] }
class B { A a }
```

如果是在多的一端没有设置belongsTo的双向一对多关联，那么级联策略将为一的一端设置为"SAVE-UPDATE" 为多的一端设置为"NONE".

+ 设置了belongsTo的单向一对一

``` groovy
class A {  }
class B { static belongsTo = [a:A] }
```

如果是设置了belongsTo的单向一对一关联，那么级联策略将为有关联的一端(A->B)设置为"ALL"，定义了belongsTo的一端(B->A)设置为"NONE".

#### 3.5.4 立即加载与延迟加载

在GORM中,关联默认是lazy的.最好的解释是例子:
```groovy
class Airport {
        String name
        static hasMany = [flights:Flight]
}
class Flight {
        String number
        static belongsTo = [airport:Airport]
}
```

上面的domain类和下面的代码:
``` groovy
def airport = Airport.findByName("Gatwick")
airport.flights.each {
        println it.name
}
```

 GORM将会执行一个单独的SQL查询来抓取 Airport 实例,然后再用一个额外的for each查询逐条迭代 flights 关联.换句话说,你得到了N+1条查询.
根据这个集合的使用频率,有时候这可能是最佳方案.因为你可以指定只有在特定的情况下才访问这个关联的逻辑.

+ 配置立即加载

一个可选的方案是使用立即抓取,它可以按照下面的方法来指定:
``` groovy
class Airport {
        String name
        static hasMany = [flights:Flight]
        static mapping = {
                flight fetch:"join"
        }
}
```

在这种情况下 Airport 实例对应的 flights 关联会被一次性全部加载进来(依赖于映射). 这样的好处是执行更少的查询,但是要小心使用,因为使用太多的eager关联可能会导致你将整个数据库加载进内存. 

+ 使用批量加载

虽然立即加载适合某些情况,它并不总是可取的,如果您所有操作都使用立即加载,那么您会将整个数据库加载到内存中,导致性能和内存的问题.替代立即加载是使用批量加载.实际上,您可以在"batches"中配置Hibernate延迟加载. 例如:
 ```groovy
class Airport {
        String name
        static hasMany = [flights:Flight]
        static mapping = {
                flight batchSize:10
        }
}
 ```
在这种情况下,由于 batchSize 参数,当您迭代 flights 关联, Hibernate 加载10个批次的结果. 例如，如果您一个 Airport 有30个s, 如果您没有配置批量加载，那么您在对Airport的查询中只能一次查询出一个结果，那么要执行30次查询以加载每个flight. 使用批量加载，您对Airport查询一次将查询出10个Flight，那么您只需查询3次. 换句话说, 批量加载是延迟加载策略的优化. 批量加载也可以配置在class级别:
 ```groovy
class Flight {
        …
        static mapping = {
                batchSize 10
        }
}
 ```

#### 3.5.5 悲观锁与乐观锁

+ 乐观锁

默认的GORM类被配置为乐观锁。乐观锁实质上是Hibernate的一个特性，它在数据库里一个特别的 version 字段中保存了一个版本号.
version 列读取包含当前你所访问的持久化实例的版本状态的  version 属性:
 ```groovy
def airport = Airport.get(10)
println airport.version
 ```

当你执行更新操作时，Hibernate将自动检查version属性和数据库中version列，如果他们不同，将会抛出一个 StaleObjectException 异常，并且当前事物也会被回滚.
这是很有用的，因为它允许你不使用悲观锁(有一些性能上的损失)就可以获得一定的原子性。由此带来的负面影响是，如果你有一些高并发的写操作的话，你必须处理这个异常。这需要刷出(flushing)当前的session:
``` groovy
def airport = Airport.get(10)
try {
        airport.name = "Heathrow"
        airport.save(flush:true)
}
catch(org.springframework.dao.OptimisticLockingFailureException e) {
        // deal with exception
}
```

你处理异常的方法取决于你的应用. 你可以尝试合并数据,或者返回给用户并让他们来处理冲突.
作为选择，如果它成了问题，你可以求助于悲观锁.

+ 悲观锁

悲观锁等价于执行一个 SQL "SELECT * FOR UPDATE" 语句并锁定数据库中的一行. 这意味着其他的读操作将会被锁定直到这个锁放开.
在Grails中悲观锁通过 lock 方法执行:
``` groovy
def airport = Airport.get(10)
airport.lock() // lock for update
airport.name = "Heathrow"
airport.save()
```

一旦当前事物被提交，Grails会自动的为你释放锁. 可是,在上述情况下我们做的事情是从正规的SELECT“升级”到SELECT ..FOR UPDATE同时其它线程也会在调用get()和lock()之间更新记录。
为了避免这个问题，你可以使用静态的lock 方法，就像get方法一样传入一个id:
 ```groovy
def airport = Airport.lock(10) // lock for update
airport.name = "Heathrow"
airport.save()
 ```

这个只有 SELECT..FOR UPDATE 时候可以使用.

> 尽管Grails和Hibernate支持悲观所，但是在使用Grails内置默认的 HSQLDB 数据库时不支持。如果你想测试悲观锁，你需要一个支持悲观锁的数据库，例如MySQL.

你也可以使用lock 方法在查询中获得悲观锁。例如使用动态查询：
``` groovy
def airport = Airport.findByName("Heathrow", [lock:true])
```

或者使用criteria:
``` groovy
def airport = Airport.createCriteria().get {
        eq('name', 'Heathrow')
        lock true
} 
```

## 4. GROM 查询

GORM提供了从动态查询器到criteria到Hibernate面向对象查询语言HQL的一系列查询方式.

+ 获取实例列表

如果你简单的需要获得给定类的所有实例，你可以使用 list 方法:`def books = Book.list()`
list 方法支持分页参数:`def books = Book.list(offset:10, max:20)`
也可以排序: `def books = Book.list(sort:"title", order:"asc")`
这里， the sort 参数是您想要查询的domain类中属性的名字，参数要么以 asc结束,要么以  desc结束.

+根据数据库标识符取回

第二个取回的基本形式是根据数据库标识符取回，使用 get 方法:
``` groovy
def book = Book.get(23)
```
你也可以根据一个标识符的集合使用 getAll方法取得一个实例列表:
 ```groovy
def books = Book.getAll(23, 93, 81)
 ```

### 4.1 动态查询

GORM支持 动态查找器 的概念 . 动态查找器看起来像一个静态方法的调用，但是这些方法本身在代码中实际上并不存在.
而是在运行时基于一个给定类的属性,自动生成一个方法. 比如例子中的 Book 类:
```groovy
class Book {
        String title
        Date releaseDate
        Author author
}                
class Author {
        String name
}
```

Book 类有一些属性，比如 title,  releaseDate 和 author. 这些都可以按照"方法表达式"的格式被用于 findBy 和 findAllBy 方法:
 ```groovy
def book = Book.findByTitle("The Stand")
book = Book.findByTitleLike("Harry Pot%")
book = Book.findByReleaseDateBetween( firstDate, secondDate )
book = Book.findByReleaseDateGreaterThan( someDate )
 ```

**方法表达式**

在GORM中一个方法表达式由前缀,比如 findBy 后面跟一个表达式组成，这个表达式由一个或多个属性组成。基本形式是:
``` groovy
Book.findBy([Property][Comparator][Boolean Operator])?[Property][Comparator]
```

用'?' 标记的部分是可选的. 每个后缀都会改变查询的性质。例如:
``` groovy
def book = Book.findByTitle("The Stand")
book =  Book.findByTitleLike("Harry Pot%")
```

在上面的例子中，第一个查询等价于等于后面的值, 第二个因为增加了 Like 后缀, 它等价于SQL的 like 表达式.
可用的后缀包括:

+ InList - list中给定的值
+ LessThan - 小于给定值
+ LessThanEquals - 小于或等于给定值
+ GreaterThan - 大于给定值
+ GreaterThanEquals - 大于或等于给定值
+ Like - 价于 SQL like 表达式
+ Ilike - 类似于Like,但不是大小写敏感
+ NotEqual - 不等于
+ Between - 于两个值之间 (需要两个参数)
+ IsNotNull - 不为null的值 (不需要参数)
+ IsNull - 为null的值 (不需要参数)

你会发现最后三个方法标注了参数的个数，他们的示例如下:
 ```groovy
def now = new Date()
def lastWeek = now - 7
def book = Book.findByReleaseDateBetween( lastWeek, now )
books = Book.findAllByReleaseDateIsNull()
books = Book.findAllByReleaseDateIsNotNull()
book = Book.findByTitleLikeOrReleaseDateLessThan( "%Something%", someDate )
 ```

**布尔逻辑**

方法表达式也可以使用一个布尔操作符来组合两个criteria:
``` groovy
def books = 
    Book.findAllByTitleLikeAndReleaseDateGreaterThan("%Java%", new Date()-30)
```
在这里我们在查询中间使用 And 来确保两个条件都满足, 但是同样地你也可以使用  Or:
``` groovy
def books = 
    Book.findAllByTitleLikeOrReleaseDateGreaterThan("%Java%", new Date()-30)
```
此时, 你最多只能用两个criteria做动态查询, 也就是说，该方法的名称只能含有一个布尔操作符. 如果你需要使用更多的, 你应该考虑使用 Criteria 或 HQL.

**查询关联**

关联也可以被用在查询中:
```groovy
def author = Author.findByName("Stephen King")
def books = author ? Book.findAllByAuthor(author) : []
```
在这里如果 Author 实例不为null 我们在查询中用它取得给定  Author 的所有Book实例.

分页和排序
跟 list 方法上可用的分页和排序参数一样,他们同样可以被提供为一个map用于动态查询器的最后一个参数:
```
def books = 
  Book.findAllByTitleLike("Harry Pot%", [max:3, 
                                         offset:2, 
                                         sort:"title",
                                         order:"desc"])
```

### 4.2 条件查询

Criteria 是一种类型安全的、高级的查询方法，它使用Groovy builder构造强大复杂的查询.它是一种比使用StringBuffer好得多的选择.
Criteria可以通过 createCriteria 或者 withCriteria 方法来使用. builder使用Hibernate的Criteria API, builder上的节点对应Hibernate Criteria API中 Restrictions 类中的静态方法. 用法示例:
```groovy
def c = Account.createCriteria()
def results = c {
        like("holderFirstName", "Fred%")
        and {
                between("balance", 500, 1000)
                eq("branch", "London")
        }
        maxResults(10)
        order("holderLastName", "desc")
}
```
**逻辑与（Conjunctions）和逻辑或（Disjunctions）**
如前面例子所演示的，你可以用 and { } 块来分组criteria到一个逻辑AND:
 ```groovy
grand {
        between("balance", 500, 1000)
        eq("branch", "London")
}
 ```

逻辑OR也可以这么做:
 ```groovy
or {
        between("balance", 500, 1000)
        eq("branch", "London")
}
 ```

你也可以用逻辑NOT来否定:
``` groovy
not {
        between("balance", 500, 1000)
        eq("branch", "London")
}
```

**查询关联**
关联可以通过使用一个跟关联属性同名的节点来查询. 比如我们说 Account 类有关联到多个  Transaction 对象:
 ```groovy
class Account {
    …
    def hasMany = [transactions:Transaction]
    Set transactions
    …
}
 ```

我们可以使用属性名 transaction 作为builder的一个节点来查询这个关联:
``` groovy
def c = Account.createCriteria()
def now = new Date()
def results = c.list {
       transactions {
            between('date',now-10, now)
       }
}
```

上面的代码将会查找所有过去10天内执行过 transactions 的  Account 实例. 你也可以在逻辑块中嵌套关联查询:
``` groovy
def c = Account.createCriteria()
def now = new Date()
def results = c.list {
     or {
        between('created',now-10,now)
        transactions {
             between('date',now-10, now)
        }
     }
}
```

这里,我们将找出在最近10天内进行过交易或者最近10天内新创建的所有用户.

**投影(Projections)查询**

投影被用于定制查询结果. 要使用投影你需要在criteria builder树里定义一个"projections"节点. projections节点内可用的方法等同于 Hibernate 的 Projections 类中的方法:
``` groovy
def c = Account.createCriteria()
def numberOfBranches = c.get {
        projections {
                countDistinct('branch')
        }
}
```
**使用可滚动的结果**

Y你可以通过调用scroll方法来使用Hibernate的 ScrollableResults 特性:
``` groovy
def results = crit.scroll {
      maxResults(10)
}
def f = results.first()
def l = results.last()
def n = results.next()
def p = results.previous()
def future = results.scroll(10)
def accountNumber = results.getLong('number')
```

下面引用的是Hibernate文档中关于ScrollableResults的描述:
 >结果集的迭代器（iterator）可以以任意步进的方式前后移动，而Query / ScrollableResults模式跟JDBC的PreparedStatement/ ResultSet也很像，其接口方法名的语意也跟ResultSet的类似.不同于JDBC，结果列的编号是从0开始.

**在Criteria实例中设置属性**

如果在builder树内部的一个节点不匹配任何一项特定标准，它将尝试设置为Criteria对象自身的属性。因此允许完全访问这个类的所有属性。下面的例子是在Criteria Criteria实例上调用  setMaxResults 和 setFirstResult:
 ```groovy
import org.hibernate.FetchMode as FM
        …
        def results = c.list {
                maxResults(10)
                firstResult(50)
                fetchMode("aRelationship", FM.EAGER)
        }
 ```
**立即加载的方式查询**
在立即加载和延迟加载 这节，我们讨论了如果指定特定的抓取方式来避免N+1查询的问题。这个criteria查询也可以做到:
``` groovy
def criteria = Task.createCriteria()
def tasks = criteria.list{
     eq "assignee.id", task.assignee.id
     join 'assignee'
     join 'project'
     order 'priority', 'asc'
}
```
注意这个 join 方法的用法. This method indicates the criteria API that a JOIN query should be used to obtain the results.

**方法引用**
如果你调用一个没有方法名的builder，比如:
``` groovy
c { … }
```

默认的会列出所有结果，因此上面代码等价于:
``` groovy
c.list { … }
```

| 方法           | 描述                                       |
| :----------- | :--------------------------------------- |
| list         | 这是默认的方法。它会返回所有匹配的行。                      |
| get          | 返回唯一的结果集，比如，就一行。criteria已经规定好了，仅仅查询一行。这个方法更方便，免得使用一个limit来只取第一行使人迷惑。 |
| scroll       | 返回一个可滚动的结果集                              |
| listDistinct | 如果子查询或者关联被使用，有一个可能就是在结果集中多次出现同一行，这个方法允许只列出不同的条目，它等价于 |
