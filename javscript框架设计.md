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

+ endWIth方法：与startWith相反
```
function endsWith(target, str, ignorecase) { 
    var end_str = target.substring(target.length - str.length); 
    return ignorecase ? end_str.toLowerCase() === str.toLowerCase() : 
            end_str === str; 
} 
```

+ repeat方法：讲一个字符串重复自身N次
版本1：空数组的join方法。
```
function repeat(target,n){
  return (new Array(n+1)).join(target);
}
```

版本 2：版本 1 的改良版，创建一个对象，拥有 length 属性，然后利用 call 方法去调用数组
原型的 join方法，省去创建数组这一步，性能大为提高。重复次数越多，两者对比越明显。另，
之所以要创建一个带 length 属性的对象，是因为要调用数组的原型方法，需要指定 call 的第一个
参数为类数组对象。而类数组对象的必要条件是其 length 属性的值为非负整数。 
```
function repeat(target,n){
  return Array.prototype.join.call({
    length : n+1
  },target);
}
```

版本 3：版本 2 的改良版，利用闭包将类数组对象与数组原型的 join方法缓存起来，省得每
次都重复创建与寻找方法。 
```
var repeat = (function() {
  var join = Array.prototype.join, obj = {};
  return function(target,n){
    obj.length = n + 1;
    return join.call(obj,target);
  }
})();
```

版本 4：从算法上着手，使用二分法，比如我们将 ruby 重复 5 次，其实我们在第二次已得
rubyruby，那么 3 次直接用 rubyruby 进行操作，而不是用 ruby。 
```
function repeat(target,n){
  var s = target, total = [];
  while(n > 0) {
    if (n % 2 ==1)
      total[total.length] = s;//如果是奇数
    if (n == 1)
      break;
    s += s;
    n = n >> 1;//相当于n除以2取其商
  }
  return total.join('');
}
```
