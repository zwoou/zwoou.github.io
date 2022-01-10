# js

## 数组  ##
方法：
1. push() 向数组的末尾添加一个或多个元素
   arrayObject.push(newelement1,......)
## ==和=== ##
``` JavaScript
var 1=='1' true
var 1==='1' false
```
## 类型 ##
1. undefined
2. boolean
3. string
4. number 
5. object 对象或null
6. function 函数

**typeof 可以用来检测给定变量的数据类型**


## 定义对象的三种方式 ##
1. 构造法： new Object()
```
var a = new Object();
a.x=1,a.y=2;
```
2. jason 构造法： 对象直接量  
```var b = { x:1,y:2 };```
3. 第三种构造法：定义类型
```
function Point(x, y){
         this.x = x;
         this.y = y;
}
var p = new Point(1,2);
```
## 函数 ##
1. Arguments 对象
在函数代码中，使用特殊对象arguments,开发者无需明确指出参数名，就能访问他们。
2. 变量的作用域
3. 几个特殊函数
    - 匿名函数
    - 回调函数
    - 自调函数
    - 内部（私有）函数
    - 返回函数的函数
## javascript中的深拷贝和浅拷贝 ##
``` javascript
obj = eval(uneval(o));
```
只能用于火狐浏览器
```
obj = JSON.parse(JSON.stringify(o));
```
效率不高
JQuery里的`.clone()`只能克隆DOM树，克隆JavaScript objects,you would do:
```
// Shallow copy
var newObject = JQuery.extend({}, oldObject);

// Deep copy
var newObject = JQuery.extend(true,{},oldObject);
```



## var、let、const区别

- var定义的变量，没有块的概念，可以跨块访问, 不能跨函数访问。有提升机制（hoisting）

- let定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问。

- const用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，而且不能修改。







