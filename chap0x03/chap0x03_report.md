# 实验三——Systemd基础入门
## 一、实验环境
+ Win 10
+ Virtualbox
+ Ubuntu 20.04 Server 64bit
## 二、实验内容
+ 确保本地已经完成asciinema auth，并在asciinema成功关联了本地账号和在线账号
+ 上传本人亲自动手完成的Systemd入门教程操作全程录像
+ 在自己的github仓库上新建markdown格式纯文本文件附上asciinema的分享URL
+ 提醒避免在终端操作录像过程中暴漏密码、个人隐私等任何机密数据
## 三、实验过程及步骤
### 1.Systemd入门教程：命令篇
- 系统管理
[![Systemd命令篇-系统管理]( https://asciinema.org/a/OCIK47RSv2MbyxHRUp6vCgvtK.svg)]( https://asciinema.org/a/OCIK47RSv2MbyxHRUp6vCgvtK)
- Unit
[![Systemd命令篇-Unit](https://asciinema.org/a/jLXSQPpcjsgDTu5Qcu2AwGMgF.svg)]( https://asciinema.org/a/jLXSQPpcjsgDTu5Qcu2AwGMgF)
- Unit的配置文件
[![Systemd命令篇-Unit的配置文件-1](  https://asciinema.org/a/bso8rQUoDBAvKA4jaE9okJPqQ.svg)](  https://asciinema.org/a/bso8rQUoDBAvKA4jaE9okJPqQ)
[![Systemd命令篇-Unit的配置文件-2](  https://asciinema.org/a/6EiLDm1ofVSTr8cJcWgFohtAg.svg)](  https://asciinema.org/a/6EiLDm1ofVSTr8cJcWgFohtAg)
- Target
[![Systemd命令篇-Target](https://asciinema.org/a/nzI2CXQQgJYmfLwE1mrviMiq1.svg)]( https://asciinema.org/a/nzI2CXQQgJYmfLwE1mrviMiq1)
- 日志管理
[![Systemd命令篇-日志管理](https://asciinema.org/a/KxJnOXHXJ1mFDtHM9Z4Tdx9cm.svg)]( https://asciinema.org/a/KxJnOXHXJ1mFDtHM9Z4Tdx9cm)
### 2.Systemd入门教程：实战篇
- 开机启动
- 启动服务
- 停止服务
- 读懂配置文件
- [Unit]区块：启动顺序与依赖关系
- [Service]区块：启动行为
- [Install]区块
- Target的配置文件
- 修改配置文件后重启
[![Systemd实战篇]( https://asciinema.org/a/BIcw1GQLMhogdayrGBDdqLjGd.svg)](https://asciinema.org/a/BIcw1GQLMhogdayrGBDdqLjGd)
### 3.自查清单
- **如何添加一个用户并使其具备sudo执行程序的权限？**
```bash
adduser <username> <username> ALL=(ALL) ALL
```
- **如何将一个用户添加到一个用户组？**
```bash
usermod -a -G <groupname> <username>
```
- **如何查看当前系统的分区表和文件系统详细信息？**
```bash
#查看分区表
sudo fdisk -l 
#以更易读的方式显示目前磁盘空间和使用情况   
df -h  
```
- **如何实现开机自动挂载Virtualbox的共享目录分区？**
在 /etc/rc.local 中用root用户执行命令：
```bash
mount -t vboxsf sharing /mnt/share
```
- **基于LVM（逻辑分卷管理）的分区如何实现动态扩容和缩减容量？**
```bash
lvextend -L +<容量> <目录>    #扩容
lvreduce -L -<容量> <目录>    #减容
```
- **如何通过systemd设置实现在网络连通时运行一个指定脚本，在网络断开时运行另一个脚本？**

    - [service]模块中设置
    ```bash
    ExecStartPost=<路径1> post1  #设置启动服务之后执行的命令
    ExecStopPost=<路径2> post2  #设置停止服务之后执行的命令
    ```
    - 重载
  
    - ``sudo systemctl daemon-reload``
    - 重启
 
    - ``sudo systemctl restart httpd.service``

- **如何通过systemd设置实现一个脚本在任何情况下被杀死之后会立即重新启动？实现杀不死？**

    - [service]区块中设置
    - ``Restart=always``

    - 重载配置文件
    - ``sudo systemctl daemon-reload``


## 四、参考文献
- [System入门教程：命令篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)
- [Systemd 入门教程：实战篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)