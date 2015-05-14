#javascript权威指南

=====
### 1.javascript 概述

##1.1 语言核心

变量与赋值
```
//单行注释

/**
* 多行注释
*/

//变量是表示值得一个符号，通过var 关键字声明
var x ;   //声明一个变量X

//通过等号赋值给变量
x = 0;  //赋值
x   //=>0:通过变量取值

//Javascript支持多种数据类型
x = 1 ;   //数字
x = 0.01; //整数与实数共用一种数据类型
x = "hello world!"; //字符串
x = 'javascript';   //字符串
x = true;       //boolean
x = false ; 
x = null;       //null是一个特殊的值，空
x = underfined; //类似null
```

对象与数组
```
//javascript 中最重要的类型是对象 对象是名/值对的集合或者字符串映射到值得集合
var book = {        //定义对象，以{}形式
  topic : "javaScript",     //属性的值
  fat : true
};

// 通过. 或者 [] 来访问对象的属性
book.topic 
book['fat']
book.author = "Flangan";  //赋值创建新属性
book.contents = {};   //{}是个空对象

//数组以数字为下标索引的列表
var primes = [2,3,4,5,7];   //以[]定义数组
primes[0]                   //第一个元素，索引为0
primes.length               //数组的长度
primes.[primes.lenth -1]    //最后一个元素
primes[5] = 9;              //添加新元素
primes[5] = 11;             //赋值
var empty = [];             //空数组
empty.length                // => 0

//数组和对象可以包含另一个数组或对象
var points = [
  {x:0,y:0},            //具有2个对象的数组
  {x:1,y:1}             //每个元素都是对象
];
var data = {                //包含两个属性的对象
  trail1 : [[1,2],[3,4]],   //每个元素的属性都是数组
  trial2 : [[2,3],[4,5]]    //数组的元素也是数组
}
```

运算符
```
// 运算符作用于操作数，生成一个新的值
3 + 2                       // =>5 加法
3 - 2                       // =>1 减法
3 * 2                       // =>5 乘法
3 / 2                       // =>1.5 除法
points[1].x - point[0].x    // =>1
"3" +　"2"                  // =>"32",+ 可以作为加法运算符，也可以作为字符串连接

//一些简写运算符
var count = 0;              //定义变量
count ++;                   //自增1
count --;                   //自减
count += 2;
count *= 3;
count                       // =>6:变量本省也是一个表达式

//相等关系运算符来判断两值是否相等，返回true或者false
var x = 2, y = 3;           
x == y                //false
x != y                //true
x <= y                //true
x < y                 //true
x >= y                //false
x > y                 //false
"two" == "three"      //false
"two" > "three"       //true: "tw"的字母索引大于"th"
false == (x > y)      //true:false与false相等

//逻辑运算符是对Boolean的合并和求反
(x == 2)&&(y == 3)      //true:与
(x > 3)||(y < 3)        //false： 或
!(x == 3)               //true:求反
```

函数
```
//函数是一段带有参数的代码段，可以被多次调用
function plus1(x){  //定义函数
  return x + 1;     //返回值
}
var y = 3;
plus1(y)        //=>4

var square = function(x) {  //函数是一种值，可以复制给变量
  return x*x;
};
square(plus(y)) //=> 16
```

函数与对象写时就变成了方法：
```
//当函数赋值给对象的属性时，成为方法，所有的javascript对象都有方法
var a = [];               //创建数组
a.push(1,2,3);            //数组的方法push：添加元素
a.reverse();              //数组的方法reverse:翻转元素

var points = [
  {x:0,y:0},            //具有2个对象的数组
  {x:1,y:1}             //每个元素都是对象
];
//定义自己的方法，this 关键字来调用
points.dist = function(){     //计算2点间距
  var p1 = this[0];           //this调用当前数组
  var p2 = this[1];
  var a = p2.x - p1.x;
  var b = p2.y - p1.y;
  return Math.sqrt(a*a + b*b);
};
points.dist()               //=>1.4142135623730951
```

控制语句
```
//判断与循环
//绝对值函数
function abs(x){
  if(x >=0 ){
    return x;
  }else{
    return -x;
  }
}

//阶乘函数
function factorial(n){
  var product = 1;
  while(n > 1){
    product *= n;
    n--;
  }
  return product;
}
factorial(4)        //=>24 : 1*4*3*2

function factorial2(n){
  var i ,product = 1;
  for (i = 2;i <= n; i ++){
    product *= i;
  }
  return product;
}
fatorial2(5)      //=>120:1*2*3*4*5
```
