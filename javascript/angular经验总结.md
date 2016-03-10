# angular 经验总结

## 1.项目的目录结构

小项目对文件按照业务逻辑进行拆分
```
controllers/
    LoginController.js
    RegistrationController.js
    ProductDetailController.js
    SearchResultsController.js
directives.js
filters.js
models/
    CartModel.js
    ProductModel.js
    SearchResultsModel.js
    UserModel.js
services/
    CartService.js
    UserService.js
    ProductService.js
```

大项目按照功能进行模块化划分目录结构
```
cart/
    CartModel.js
    CartService.js
common/
    directives.js
    filters.js
product/
    search/
        SearchResultsController.js
        SearchResultsModel.js
    ProductDetailController.js
    ProductModel.js
    ProductService.js
user/
    LoginController.js
    RegistrationController.js
    UserModel.js
    UserService.js
```

##2.数据的双向绑定的原理

Angular实现了双向绑定机制。所谓的双向绑定，无非是从界面的操作能实时反映到数据，数据的变更能实时展现到界面。
Angular使用脏检查机制(dirty-checking)：当改变数据之后，你自己要做一些事情来触发脏检测，然后再应用到这个数据对应的DOM元素上。

ng只有在指定事件触发后，才进入$digest cycle：
  1. DOM事件，譬如用户输入文本，点击按钮等。(ng-click)
  2. XHR响应事件 ($http)
  3. 浏览器Location变更事件 ($location)
  4. Timer事件($timeout, $interval)
  5. 执行$digest()或$apply()
  
angular对常用的dom事件，xhr事件等做了封装， 在里面触发进入angular的digest流程。在digest流程里面， 会从rootscope开始遍历， 检查所有的watcher。

### 2.1 怎样触发脏检测？什么时候触发？

一些基于setter的框架，它可以在给数据设值的时候，对DOM元素上的绑定变量作重新赋值。
脏检测的机制没有这个阶段，它没有任何途径在数据变更之后立即得到通知，所以只能在每个事件入口中手动调用apply()，把数据的变更应用到界面上。
在真正的Angular实现中，这里先进行脏检测，确定数据有变化了，然后才对界面设值。
所以，在ng-click里面封装真正的click，最重要的作用是为了在之后追加一次apply()，把数据的变更应用到界面上去。

### 2.2 $digest和$apply

在Angular中，有$apply和$digest两个函数，我们刚才是通过$digest来让这个数据应用到界面上。

区别：
最直接的差异是，$apply可以带参数，它可以接受一个函数，然后在应用数据之后，调用这个函数。所以，一般在集成非Angular框架的代码时，可以把代码写在这个里面调用。

在简单的数据模型中，这两者没有本质差别，但是当有层次结构的时候，就不一样了。考虑到有两层作用域，我们可以在父作用域上调用这两个函数，也可以在子作用域上调用，这个时候就能看到差别了。
对于$digest来说，在父作用域和子作用域上调用是有差别的，但是，对于$apply来说，这两者一样。

当调用$digest的时候，只触发当前作用域和它的子作用域上的监控，但是当调用$apply的时候，会触发作用域树上的所有监控。

因此，从性能上讲，如果能确定自己作的这个数据变更所造成的影响范围，应当尽量调用$digest，只有当无法精确知道数据变更造成的影响范围时，才去用$apply，很暴力地遍历整个作用域树，调用其中所有的监控。

从另外一个角度，我们也可以看到，为什么调用外部框架的时候，是推荐放在$apply中，因为只有这个地方才是对所有数据变更都应用的地方，如果用$digest，有可能临时丢失数据变更。


## 3.视图模型的层次

### 3.1 嵌套作用域的数据继承

在Angular中，存在作用域的继承。所谓作用域的继承，是指：如果两个视图有包含关系，内层视图对应的作用域可以共享外层视图作用域的数据。

其实是通过原型继承来做到这样的。内层scope的prototype被自动设置为外层的scope了，所以，才可以在内层使用这个a。这时候在内层给a赋值，当然就赋到它自己上了，不会赋值到原型的那个对象上。

同理，如果内外两层作用域上存在同名变量，在内层界面赋值的时候只会赋到内层作用域上的那个变量，不会影响到外层的。

### 3.2 引入视图模型的继承

1. ng-controller 的嵌套
2. 数组和对象属性的迭代:ng-repeat
3. 动态包含 比如动态引入界面模板ng-include 或者 ng-view

### 使用注意

在AngularJS中，视图模型的继承虽然使得很多时候代码写起来比较方便，但有些时候会造成很多麻烦。
当编写视图模型代码的时候，应当尽量避免父子作用域存在同名变量的情况，以防止造成隐含的问题。