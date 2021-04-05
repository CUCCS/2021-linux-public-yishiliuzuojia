## 实验要求
+ 在asciinema注册一个账号，并在本地安装配置好asciinema
+ 确保本地已经完成asciinema auth，并在asciinema成功关联本地账号和在线账号
+ 上传本人亲自动手完成的vimtutor操作全程录像
+ vimtutor完成自查清单
## 实验步骤
### 1.打开虚拟机，在Ubuntu系统中安装asciinema
```
sudo apt-add-repository ppa:zanchey/asciinema
sudo apt-get update
sudo apt-get install asciinema
```
### 2.将asciinema与本地帐号以及在线账号相关联
`` asciinema auth``
### 3.键入以下命令开始录制
`` asciinema rec``
### 4.学习vimtutor
+ LESSON 1
[![asciicast](https://asciinema.org/a/XhBjnrWhaUYVAgk0EQJAtQtbn.svg)](https://asciinema.org/a/XhBjnrWhaUYVAgk0EQJAtQtbn)
+ LESSON 2
[![asciicast](https://asciinema.org/a/zJPCBztNSb27ZQAsNsSmxaCwD.svg)](https://asciinema.org/a/zJPCBztNSb27ZQAsNsSmxaCwD)
+ LESSON 3
[![asciicast](https://asciinema.org/a/Bd8A4e5m7DzNQYzNkfqX2P9N8.svg)](https://asciinema.org/a/Bd8A4e5m7DzNQYzNkfqX2P9N8)
+ LESSON 4
[![asciicast](https://asciinema.org/a/ok8Ho819Hc5DUTPJsrxihIUVp.svg)]( https://asciinema.org/a/ok8Ho819Hc5DUTPJsrxihIUVp)
+ LESSON 5
[![asciicast](https://asciinema.org/a/VXyyx9hEqqzvyFdqR0wH2SGXR.svg)](https://asciinema.org/a/VXyyx9hEqqzvyFdqR0wH2SGXR)
+ LESSON 6
[![asciicast](https://asciinema.org/a/4oQCwJCzGxFMNQOGm7rM7LfoH.svg)](https://asciinema.org/a/4oQCwJCzGxFMNQOGm7rM7LfoH)
+ LESSON 7
[![asciicast](https://asciinema.org/a/2lZPHvG0eOxOE9jRqqEHSSw95.svg)](https://asciinema.org/a/2lZPHvG0eOxOE9jRqqEHSSw95)
### 5.自查清单
##### (1)你了解vim有哪几种工作模式？
+ normal-mode
+ insert-mode
+ visual-mode
+ command-mode
##### (2)Normal模式下，从当前行开始，一次向下移动光标10行的操作方法？如何快速移动到文件开始行和结束行？如何快速跳转到文件中的第N行？
+ 通过10+j
+ 通过gg快速移动到文件开始行；通过G快速移动到文件结束行
+ 通过行数+G快速跳转到文件中的第N行
##### (3)Normal模式下，如何删除单个字符、单个单词、从当前光标位置一直删除到行尾、单行、当前行开始向下数N行？
+ 通过x删除单个字符
+ 通过dw删除单个单词
+ 通过d$从当前光标位置一直删除到行尾
+ 通过dd删除单行
+ 通过行数+dd删除当前行开始向下数N行
##### (4)如何在vim中快速插入N个空行？如何在vim中快速输入80个-？
+ 通过N+o/O在vim中快速插入N个空行
+ 通过80i- + esc快速输入80个-
##### (5)如何撤销最近一次编辑操作？如何重做最近一次被撤销的操作？
+ 通过u撤销最近一次编辑操作
+ 通过CTRL+R重做最近一次被撤销的操作
##### (6)vim中如何实现剪切粘贴单个字符？单个单词？单行？如何实现相似的复制粘贴操作呢？
+ 光标放置在想要剪切的字符上按x，再放在想要粘贴的位置前面按p
+ 光标放置在想要剪切的单词首字母上按dw，再放在想要粘贴的位置前面按p
+ 光标放在想要剪切的单行上按dd，再放在想要粘贴的位置按p
+ 按v进入visual-mode，移动光标进行选择，按y进行复制，按p进行粘贴
##### (7)为了编辑一段文本你能想到哪几种操作方式（按键序列）？
+ 按x删除文本，按i输入文本，按p粘贴删除的文本，按r替换要修改的文本，按a在光标位置之后插入文本，按A在光标所在行的行末插入文本
##### (8)查看当前正在编辑的文件名的方法？查看当前光标所在行的行号的方法？
+ 通过CTRL+G
##### (9)在文件中进行关键词搜索你会哪些方法？如何设置忽略大小写的情况下进行匹配搜索？如何将匹配的搜索结果进行高亮显示？如何对匹配到的关键词进行批量替换？
+ /关键词 进行检索
+ :set ic 忽略大小写
+ :set hls is 进行高亮显示
+ :%s/被替换词/替换词/g 替换所有被替换词
##### (10)在文件中最近编辑过的位置来回快速跳转的方法？
+ CTRL+O向前跳转
+ CTRL+I向后跳转
##### (11)如何把光标定位到各种括号的匹配项？例如：找到(, [, or {对应匹配的),], or }
+ 光标放在括号上，通过%找到对应的括号
##### (12)在不退出vim的情况下执行一个外部程序的方法？
+ :!+外部程序名
##### (13)如何使用vim的内置帮助系统来查询一个内置默认快捷键的使用方法？如何在两个不同的分屏窗口中移动光标？
+ :help+快捷键名称
+ ctrl + w + h 向右
+ ctrl + w + j 向下
+ ctrl + w + k 向上
+ ctrl + w + l 向左