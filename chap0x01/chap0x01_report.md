# 实验问题
+ 如何配置无人值守安装iso并在Virtualbox完成自动化安装
+ Virtualbox安装完Ubuntu之后新添加的网卡如何实现系统开机自动启用和自动获取IP？
+ 如何使用sftp在虚拟机和宿主机之间传输文件？

# 主要操作步骤
### 1.提前下载好纯净版 Ubuntu 安装镜像 iso 文件
![img](img/1.png)

### 使用手动安装 Ubuntu 
![img](img/手动安装Ubuntu1.png)
![img](img/手动安装Ubuntu2.png)
![img](img/手动安装Ubuntu3.png)
![img](img/手动安装Ubuntu4.png "双网卡")
![img](img/手动安装Ubuntu5.png "安装镜像")

##### 默认英语
![img](img/默认英语.png)

##### 检测网卡
![img](img/2.png "检测网卡")

##### 用户名设置
![img](img/用户名设置.png)

##### 开始安装
![img](img/手动开始安装.png)

##### 安装完成
![img](img/安装完成.png)

### 2.新建可以用于安装 Ubuntu 64位系统的虚拟机配置
![img](img/3.png)

### 3.制作包含user-data 和 meta-data 的 ISO 镜像文件，命名为 vicky-focal-init.iso，我设置的meta-data文件为空

##### 查看Ubuntu的IP地址
![img](img/查看IP.png)

##### 利用SSH
![img](img/SSH.png)

##### 使用sftp将user-data和meta-data传输到虚拟机里
![img](img/5.png)

##### 在虚拟机中看到两个文件已经成功上传
![img](img/6.png)

##### 软件包更新
![img](img/sudo软件包更新.png)

##### 用以下代码安装无人值守相关依赖工具
`` # ubuntu ``

``sudo apt install genisoimage``


##### 创建vicky-focal-init.iso镜像
``genisoimage -output vicky-focal-init.iso -volid cidata -joliet -rock user-data meta-data``

![img](img/7.png)

##### 将刚刚制作成功的iso镜像文件保存到主机D盘根目录
![img](img/4.png)
![img](img/8.png)

### 4.移除上述虚拟机「设置」-「存储」-「控制器：IDE」
![img](img/移除控制器.jpg)

### 5.在「控制器：SATA」下新建 2 个虚拟光盘

##### 按顺序 先挂载「纯净版 Ubuntu 安装镜像文件」

![img](img/添加虚拟光驱.jpg)
![img](img/5.jpg "挂载纯净版Ubuntu安装镜像文件")

##### 后挂载 focal-init.iso
![img](img/再次添加虚拟光驱.jpg)
![img](img/9.png)
![img](img/10.png)

##### 第二块网卡设置成host-only
![img](img/11.png)

### 6.「无人值守安装」程序自动完成系统安装和重启进入系统可用状态

##### 确认好存储设置并启动虚拟机
![img](img/确认存储设置.png)
![img](img/12.png)

### 学习sftp的参考链接
<https://www.cnblogs.com/afeige/p/12144296.html>
