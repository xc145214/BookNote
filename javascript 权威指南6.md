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
