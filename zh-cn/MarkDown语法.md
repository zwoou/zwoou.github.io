---
title: MarkDown语法
tags:
  - markdown

---

# 语法 #
## 段落 ##
另起一行需要段落之间空一行

插入换行标签 需要两个空格  
## 粗体
### 删除线
## 标题 ##
	# 这是H1 #
## 引用 ##
----

<!--more-->
> 这是引用  
>> 接着引用
## 列表 ##
无序列表使用星号、加号或减号做列标记  

* Red  
* Green  
* Blue  
+ Red
+ Green  
+ Blue  
- Red  
- Blue  
- Green

有序列表使用数值+英文句点  
## 内联代码 ##
用反引号'来标记内联代码  
## 代码区域 ##
github的风格  
'''
这是一个代码区块
'''  
    代码区块
## 分割线 ##
* * *
------
- - -
## 链接 ##
[an example](http://example.com/)
[an example](http://example.com/ "Optional Title")  
会被解释为  
    <a href='http://example.com/'>an example</a>
    <a href='http://example.com/' title="Optional Title">an example</a>
## 图像 ##
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional Title")
![shiro](http://ouzl5aeyt.bkt.clouddn.com/Shiro.png)   
会被解释为  
<img src='/path/to/img.jpg' alt='Alt text' />  
<img src='/path/to/img.jpg' alt='Alt text' title='Optional Title' />
## 自动链接 ##
如果链接的地址和名字重复，可以用尖括号语法将其简化。

<http://example.com/>

就相当于

[http://example.com/](http://example.com/)

切记，大多数编辑器都会自动将符合url规则的东西视为链接，并且解释成链接。很多时候作者由于疏忽等缘故，链接和后面的中文之间缺少空格，导致链接不正常。所以我建议，链接要么加上尖括号，要么两端加上空格。  
## 转义 ##
Markdown中的转义字符为\，可以转义的有：

- \\ 反斜杠
- *\` 反引号
- \* 星号
- \_ 下划线
- \{\} 大括号
- \[\] 中括号
- \(\) 小括号
- \# 井号
- \+ 加号
- \- 减号
- \. 英文句号
- \! 感叹号
## 表格 ##
表格是github风格独有的语法，但近年来渐渐被大多数编辑器支持。

| Item     | Value | Qty   |
| :------- | ----: | :---: |
| Computer | $1600 |  5    |
| Phone    | $12   |  12   |
| Pipe     | $1    |  234  |
会被解释成
<table>
<thead>
<tr>
  <th align="left">Item</th>
  <th align="right">Value</th>
  <th align="center">Qty</th>
</tr>
</thead>
<tbody><tr>
  <td align="left">Computer</td>
  <td align="right">$1600</td>
  <td align="center">5</td>
</tr>
<tr>
  <td align="left">Phone</td>
  <td align="right">$12</td>
  <td align="center">12</td>
</tr>
<tr>
  <td align="left">Pipe</td>
  <td align="right">$1</td>
  <td align="center">234</td>
</tr>
</tbody></table>

## 内联HTML ##
markdown 的语法简洁，但有其局限性，所以特意保留了内联html这种方式。任何html标签及其内容，都会原样输出到结果中。也就是说，标签中的星号等作为markdown结构的符号，以及构成html标签和实体的符号，都不会做任何转义。
