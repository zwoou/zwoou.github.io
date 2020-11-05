# vim使用

## 设置行号

- `：set number` 显示行号
- `：set nonumber` 不显示行号

## 查找字符串

- 从开头搜索

在命令模式下，输入/你要查找的字符

按下回车，可以看到vim把光标移动到该字符处

再按n（小写）查看下一个匹配

按N(大写）查看上一个匹配，

capslock切换大小写，也可以在小写状态下按shift+n

- 从结尾处搜索

？要搜索的字符串或字符

搜索后，打开别的文件发现也被高亮了，怎么关闭

命令行模式下，输入：nohlsearch

也可以：set nohlsearch

https://gitee.com/zwoou/picgo/raw/master/pic/vi-vim-cheat-sheet-sch.gif

![](C:\Users\admin\Google 云端硬盘\zwoou.github.io\vim.assets\vi-vim-cheat-sheet-sch.gif)