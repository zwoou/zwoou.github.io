# node.js

## node安装

查看 `node -v`

## NPM包管理工具

- 查看
```
$ npm -v
```
- 安装模块
```
$ npm install <Model Name>
```
安装Web模块
```
$ npm install express
```
使用淘宝镜像命令
```
cnpm install npm -g
```
- 全局安装和本地安装
```
$ npm install express    # 本地安装
$ npm install express -g # 全局安装
```
<!-- more -->
本地安装会在运行命令所在目录生成./node_modules
- 查看安装信息
```
$ npm list -g
```
- 卸载模块
```
$ npm uninstall express
```
卸载后
```
$ npm ls
```
- 更新模块
```
$ npm update express
```
- 搜索模块
```
$ npm search express
```
- 创建模块
```
$ npm init
```
## 设置代理

为npm设在代理
```
npm config set proxy="http://192.168.1.1:8080"
```
为npm默认选择http方式，不选用https
```
npm config set registry http://registry.npmjs.org
```
## 使用package.json
package.json 位于模块的目录下，用于定义包的属性。接下来让我们来看下 express 包的 package.json 文件，位于 node_modules/express/package.json 内容：
## 使用淘宝NPM镜像
```
 npm install -g cnpm --registry=https://registry.npm.taobao.org
```
这样就可以使用 cnpm 命令来安装模块了：
```
$ cnpm install [name]
```
## 回调函数
四个概念：同步、异步、阻塞、非阻塞
异步编程的直接体现是回调
，回调函数的参数一般作为最后一个参数出现。
## 事件循环
Node.js 是单进程单线程应用程序
Node.js 几乎每一个API都是支持回调函数的。
Node.js 基本上所有的事件机制都是用设计模式中的观察者模式实现。
### 事件驱动程序
Node.js使用事件驱动模型，当web server 接收到请求，就把他关闭然后进行处理，然后去服务下一个web请求。
当这个请求完成，他被放回处理队列，到达队列开头，这个结果返回给用户。
在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件触发回掉函数。
## EventEmitter类
events模块只提供一个了一个对象：events.EventEmitter.EventEmitter的核心就是事件触法与事件监听器功能的封装。
- error 事件
- 继承EventEmitter事件
## Buffer（缓冲区）
js语言自身只有字符串数据类型，没有二进制数据类型，在处理像TCP流或文件流时，必须要用二进制数据，因此使用Buffer
V6.0之后创建Buffer对象，使用Buffer.from()接口创建。
- Buffer与字符编码 如UTF-8、UCS2、Base64或十六进制编码


## Stream(流)
Stream是一个抽象接口，Node中有很多对象实现了这个接口。
Stream有四种类型
	- Readable 可读操作
	- Writable 可写操作
	- Duplex 可读可写操作
	- Transform 操作被写入数据，然后读出结果。
所有的Stream对象都是EventEmitter的实例，常用的有
    - data 当有数据可读时触发
    - end 没有更多的数据可读时触发
    - error 在接收和写入过程中发生错误时触发。
    - finish 所有数据已被写入到底层系统时触发。
- 管道流
管道流提供了一个输出流到输入流的机制。
- 链式流
链式是通过连接输出流到另外一个流并创建多个流操作链的机制。
## 模块系统
创建模块两种方式
	- require 用于从外部获取一个模块的接口
	- exports 是模块公开的接口
- Node.js 中存在4类模块
	 - 文件模块缓存区
	 - 原生模块缓存区
	 - 原生模块
	 - 文件模块
## 函数
一个函数可以作为另一个函数的参数。
- 匿名函数
## 路由
Node.js模块，url和querystring

## 全局对象
全局对象（Global Object），js中window是全局对象，而Node.js中是 global
- __filename 当前正在执行的脚本的文件名。输出文件所在位置的绝对路径。
- __dirname 当前执行脚本所在的目录。
- setTimeout(cb,ms) 在指定的毫秒数后执行的函数（cb).
## 常用工具
- util.inherits
util.inherits(constructor, superConstructor) 是一个实现对象间原型继承的函数
js的面向对象特征是基于原型的，与常见的基于类不同。
- util.inspect 
util.inspect(object,[showHidden],[depth],[colors]) 将任意对象转换成字符串，通常用于调试和错误输出。
## 文件系统
异步和同步
	异步方法效率高
## 工具模块
- OS 提供基本的系统操作函数
- Path 提供了处理和转换文件路径的工具
- Net 用于底层的网络通信
- DNS 用于解析域名
- Domain 简化异步代码的异常处理
## Express 框架
一个简洁而灵活的web应用框架。
特性：
    - 可以设置中间件来响应 HTTP 请求。

    - 定义了路由表用于执行不同的 HTTP 请求动作。
    
    - 可以通过向模板传递参数来动态渲染 HTML 页面。

参考
- [https://npm.taobao.org/](https://npm.taobao.org/)
- [https://www.runoob.com/nodejs/nodejs-npm.html](https://www.runoob.com/nodejs/nodejs-npm.html)
	
