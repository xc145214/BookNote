# angular 经验总结

+ ##1.项目的目录结构

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

+ ##2.数据的双向绑定的原理

Angular实现了双向绑定机制。所谓的双向绑定，无非是从界面的操作能实时反映到数据，数据的变更能实时展现到界面。
Angular使用脏检查机制(dirty-checking)：当改变数据之后，你自己要做一些事情来触发脏检测，然后再应用到这个数据对应的DOM元素上。

ng只有在指定事件触发后，才进入$digest cycle：
  1. DOM事件，譬如用户输入文本，点击按钮等。(ng-click)
  2. XHR响应事件 ($http)
  3. 浏览器Location变更事件 ($location)
  4. Timer事件($timeout, $interval)
  5. 执行$digest()或$apply()
  
angular对常用的dom事件，xhr事件等做了封装， 在里面触发进入angular的digest流程。在digest流程里面， 会从rootscope开始遍历， 检查所有的watcher。
