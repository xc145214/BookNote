#javascript 框架设计
====
## 前言

###1. 框架与库

+ 框架
框架式一个半成品的应用，直接给出骨架，按规范编写模块。比如angular

+ 库
库是解决某个问题而拼凑出来的一大堆函数与类的集合。比如jquery

> 无论是框架还是库，只要在浏览器中运行，就要与DOM打交道。

###2.JavaScript的发展

+ 洪荒时期 代表Dojo,bindows
+ Prototype王朝 
+ jquery纪元

###3. Javascript框架分类

+ 以命名空间为导向的类库或者框架：完全的Java风格，代表作有YUI和EXT
+ 以类工厂为导向的框架：比如Prototype，mootools,Base2等
+ 以选择起为导向的框架：比如Jquery
+ 已加载器串联起来的框架：比如模块化的AMD，比如kissy mass
+ 具有明确分层构架的MV* 框架。比如CanJS backBonesjs spinejs及angular等

  > jquery 特性：get first set all 访问规则； 数据缓存系统；无实例化计数，不需要new


###2.语言模块

####2.1 字符串的扩展与修复
具体实现：

+ contains方法：判断一个字符串是否包括另一个字符串。常规思维使用正则，但是每次都要new RegExp来构造，性能太差，转而使用原生字符串的方法比如：indexOf search
```
function contains(target,it){
  return target.indexOf(it) != -1;//indexOf改为search或者lastIndexOf也行
}
```
mootools版本
```
function contains(target, str, separator) { 
    return separator ? 
            (separator + target + separator).indexOf(separator + str + separator) > -1 : 
            target.indexOf(str) > -1; 
} 
```

+ startWith方法：判断字符串是否位于原字符串的开始之处，contains变种
```
//最后一参数是忽略大小写 
function startsWith(target, str, ignorecase) { 
    var start_str = target.substr(0, str.length); 
    return ignorecase ? start_str.toLowerCase() === str.toLowerCase() : 
            start_str === str; 
}
```
