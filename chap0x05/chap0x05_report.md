# 实验报告

## 实验环境

- 测试客户端

    * Windows 10 IP:192.168.56.106

- 服务端

    * Ubuntu 20.04 IP:192.168.56.103

      |           | 端口 |
      | :-------: | :--: |
      | verynginx |  80  |
      |   nginx   | 8080 |
      | wordpress | 8081 |
      |   dvwa    | 8082 |



    * Nginx 1.18.0

    * VeryNginx

    * WordPress 4.7

    * Damn Vulnerable Web Application (DVWA)

- 更改Windows主机```C:\Windows\System32\drivers\etc```的hosts文件和虚拟机的hosts文件

	```
	192.168.56.103 vn.sec.cuc.edu.cn
	192.168.56.103 dvwa.sec.cuc.edu.cn
	192.168.56.103 wp.sec.cuc.edu.cn
	```

## 实验过程

#### 基本要求

**1. 在虚拟机中国配置Nginx和VeryNginx**

- 安装Nginx

    ```bash
    # 安装Nginx
    sudo apt update && sudo apt install nginx
    # 查看Nginx的版本
    nginx -V
    ```

- 安装VeryNginx

    ```bash
    # 安装git
    sudo apt install git
    
    # 克隆VeryNginx仓库到Linux
    git clone https://github.com/alexazhou/VeryNginx.git
    
    # 安装相关依赖包
    sudo apt-get install libssl-dev
    sudo apt install libpcre3 libpcre3-dev
    sudo apt install build-essential
    sudo apt install zlib1g-dev
    
    # 进入仓库
    cd VeryNginx/
    
    # 安装VeryNginx
    sudo python3 install.py install
    ```

- 修改配置文件

  1. 在```/opt/verynginx/openresty/nginx/conf/nginx.conf```中，将```user  nginx```改成```user www-data```

  2. 修改监听端口为``192.168.56.103:80``

  4. 修改Nginx配置

     ```bash
     sudo vim /etc/nginx/sites-available/default
     
     # 将端口修改为8080
     listen 8080 default_server;
     listen [::]:8080 default_server;
     ```

- nginx服务

  ```bash
  # 启动服务
  sudo /opt/verynginx/openresty/nginx/sbin/nginx
  
  #停止服务
  sudo /opt/verynginx/openresty/nginx/sbin/nginx -s stop
  
  #重启服务
  sudo /opt/verynginx/openresty/nginx/sbin/nginx -s reload
  ```

- 登录管理界面

  * 成功连接

    ![](img/成功.jpg)

  * 浏览器访问

    网址：```192.168.56.103/verynginx/index.html```

    用户名：```verynginx```

    密码：```verynginx```

    ![](img/verynginx截图.jpg)

- 下载mysql用于数据管理

  * 安装mysql

    ```bash
    sudo apt install mysql-server
    ```

  * 检查是否正常运行

    ```bash
    sudo mysql -u root -p #密码是cuc
    
    # exit退出
    ```

- php

  * 安装php

    ```bash
    sudo apt install php-fpm php-mysql php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip
    ```

  * 安装成功,提示信息

    ```bash
    NOTICE: Not enabling PHP 7.4 FPM by default.
    NOTICE: To enable PHP 7.4 FPM in Apache2 do:
    NOTICE: a2enmod proxy_fcgi setenvif
    NOTICE: a2enconf php7.4-fpm
    NOTICE: You are seeing this message because you have apache2 package installed.
    ```

  * PHP-FPM进程的反向代理配置在nginx服务器上

    ```bash
    # 修改nginx配置文件
    sudo vim /etc/nginx/sites-enabled/default
    
    # 取消掉以下内容的注释
    location ~ \.php$ {
  	    include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
    }
    ```

**2. WordPress**

- 安装WordPress

  ```bash
  # 下载安装包
  sudo wget https://wordpress.org/wordpress-4.7.zip
  # 下载解压软件
  sudo apt install unzip
  # 解压
  unzip wordpress-4.7.zip
  # 将解压后的wordpress移到指定路径
  sudo mkdir /var/www/html/wp.sec.cuc.edu.cn
  sudo cp -r  home/cuc/wordpress /var/www/html/wp.sec.cuc.edu.cn/
  ```

- 配置WordPress

  * 进入```/var/www/html/wp.sec.cuc.edu.cn/wordpress```目录下修改文件

    ```bash
    sudo vim wp-config-sample.php
    ```

  * 补充数据库信息

    ```bash
    # 登录
    sudo mysql
    # 建库
    CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
    # 创建用户
    create user 'cuc'@'localhost' identified by 'cuc';
    # 授权
    grant all on wordpress.* to 'cuc'@'localhost';
    # 退出
    exit
    ```

  * 修改内容
    ```bash
    // ** MySQL settings - You can get this info from your web host ** //
    /** The name of the database for WordPress */
    define('DB_NAME', 'wordpress');
    
    /** MySQL database username */
    define('DB_USER', 'cuc');
    
    /** MySQL database password */
    define('DB_PASSWORD', 'cuc');
    
    /** MySQL hostname */
    define('DB_HOST', 'localhost');
    
    /** Database Charset to use in creating database tables. */
    define('DB_CHARSET', 'utf8mb4');
    
    /** The Database Collate type. Don't change this if in doubt. */
    define('DB_COLLATE', '');
    ```
- 创建ngnix下的配置文件
  * 复制文件
    ```bash
    sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/wp
    ```
  * 配置文件
    ```bash
    sudo vim /etc/nginx/sites-available/wp
    # 修改监听端口
    listen 8081 default_server;
    listen [::]:8081 default_server;
      
    # 修改网站根站点，为wordpress的安装目录
    root /var/www/html/wp.sec.cuc.edu.cn/wordpress;
      
    # 修改server_name
    server_name wp.sec.cuc.edu.cn;
    
    # 添加index.php
    index index.php index.html index.htm index.nginx-debian.html;
    ```
  * 建立连接
    ```bash
    # 语法检查    
    sudo nginx -t
    
    # 建立软链接
    sudo ln -s /etc/nginx/sites-available/wp /etc/nginx/sites-enabled/
    
    # 启动nginx
    sudo systemctl restart nginx
    ```
**3. DVWA**
- 安装DVWA
  ```bash
  # 下载DVWA
  git clone https://github.com/digininja/DVWA.git
  
  # 建立存放的目录
  sudo mkdir /var/www/html/dvwa.sec.cuc.edu.cn
  
  # 把下载好的DVWA移到刚刚创建的目录下
  cd home/cuc
  sudo mv DVWA/* /var/www/html/dvwa.sec.cuc.edu.cn
  
  # 修改文件夹属主为 www-data
  sudo chown -R www-data:www-data /var/www/html/dvwa.sec.cuc.edu.cn
  ```
  
- 配置MySQL
  ```bash
  # 启动MySQL
  sudo mysql
  
  # 建立dvwa的数据库
  create database dvwa;
  
  # 创建用户
  create user dvwa@localhost identified by 'p@ssw0rd';
  
  # 授权
  grant all on dvwa.* to dvwa@localhost;
  
  # 刷新权限
  flush privileges;
  
  # 退出
  exit
  
  # 重启mysql使配置文件生效
  sudo systemctl restart mysql
  ```
- 配置php
  * 修改文件名字
    ```bash
    # 将/var/www/html/dvwa.sec.cuc.edu.cn/config/目录下的config.inc.php.dist文件改名为config.inc.php
    sudo mv config.inc.php.dist config.inc.php
    ```
  * 修改php-fpm文件
    ```bash
    sudo vim /etc/php/7.4/fpm/php.ini
    
    # 修改内容
    allow_url_include = On
    ```
  * 重启php
    ```bash
    sudo systemctl restart php7.4-fpm.service
    ```
  * 配置文件
    ```bash
    # 创建ngnix下的配置文件
    sudo cp /etc/nginx/sites-available/wp /etc/nginx/sites-available/dvwa
    
    # 修改nginx配置
    sudo vim /etc/nginx/sites-available/dvwa
    
    # 修改内容
    listen 8082 default_server;
    listen [::]:8082 default_server;
    
    root /var/www/html/dvwa.sec.cuc.edu.cn;
    
    server_name dvwa.sec.cuc.edu.cn;
    
    index index.php index.html setup.php index.htm  index.nginx-debian.html;
    ```
  * 建立连接
    ```bash 
    # 语法检查
    sudo nginx -t
    
    # 创建软链接
    sudo ln -s /etc/nginx/sites-available/dvwa /etc/nginx/sites-enabled/
    
    # 重启nginx
    sudo systemctl restart nginx
    ```
- 成功登录
  * 登录dvwa
    用户名：```dvwa```
    密码：```p@ssw0rd```
  * 创建数据库
    点击```login.php```页面下方的```Create/Reset Database```生成需要使用的数据库。如果数据库连接成功，页面会直接重定向到登录页面，此时使用```admin/password```登录
**4. 配置WordPress和DVWA**
- WordPress
- DVWA
####  安全加固要求
**1. 使用IP地址方式均无法访问上述任意站点，并向访客展示自定义的友好错误提示信息页面-1**
- 添加mathcer
  ```bash
  # 先修改目录权限
  sudo chown -R www-data:www-data /opt/verynginx/verynginx/configs
  ```
**2. Damn Vulnerable Web Application (DVWA)只允许白名单上的访客来源IP，其他来源的IP访问均向访客展示自定义的友好错误提示信息页面-2**
- 添加matcher
- 添加response
- 添加filter
**3. 在不升级Wordpress版本的情况下，通过定制VeryNginx的访问控制策略规则，热修复WordPress < 4.7.1 - Username Enumeration**
- 添加matcher
- 添加filter
- 添加后访问失败，返回404
**4. 通过配置VeryNginx的Filter规则实现对Damn Vulnerable Web Application (DVWA)的SQL注入实验在低安全等级条件下进行防护**
- 登录DVWA，选择```DVWA Security```中的```Security Level```为```Low```，点击```submit```
- 然后选择```sql injection```，进行sql注入
- 添加matcher
- 添加response
- 添加filter
#### VeryNginx配置要求
**1. VeryNginx的Web管理页面仅允许白名单上的访客来源IP，其他来源的IP访问均向访客展示自定义的友好错误提示信息页面-3**
**2.通过定制VeryNginx的访问控制策略规则实现：**
- 限制DVWA站点的单IP访问速率为每秒请求数 < 50
- 限制Wordpress站点的单IP访问速率为每秒请求数 < 20
  * 添加response
  * 添加频率限制信息
- 超过访问频率限制的请求直接返回自定义错误提示信息页面-4
  ```bash
  # 下载包以使用ab进行测试
  sudo apt update
  sudo apt install apache2-utils 
  ```
  * 测试wp.sec.cuc.edu.cn
    ```bash
    ab -n 100 http://wp.sec.cuc.edu.cn/
    ```
    ```bash
    Server Software:        openresty/1.15.8.1
    Server Hostname:        wp.sec.cuc.edu.cn
    Server Port:            80
    
    Document Path:          /
    Document Length:        162 bytes
    
    Concurrency Level:      1
    Time taken for tests:   0.264 seconds
    Complete requests:      100
    Failed requests:        80
       (Connect: 0, Receive: 0, Length: 80, Exceptions: 0)
    Non-2xx responses:      20
    Total transferred:      19800 bytes
    HTML transferred:       4840 bytes
    Requests per second:    293.47 [#/sec] (mean)
    Time per request:       2.638 [ms] (mean)
    Time per request:       2.638 [ms] (mean, across all concurrent requests)
    Transfer rate:          73.29 [Kbytes/sec] received
    
    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        1    1   1.7      1      14
    Processing:     0    1   1.9      1      14
    Waiting:        0    1   1.9      1      14
    Total:          1    3   2.7      2      16
    
    Percentage of the requests served within a certain time (ms)
      50%      2
      66%      2
      75%      2
      80%      3
      90%      5
      95%      7
      98%     15
      99%     16
     100%     16 (longest request)
    ```
    
  * 测试dvwa.sec.cuc.edu.cn
    ```bash
    ab -n 100 http://dvwa.sec.cuc.edu.cn/
    ```
    ```bash
    Server Software:        openresty/1.15.8.1
    Server Hostname:        dvwa.sec.cuc.edu.cn
    Server Port:            80
    
    Document Path:          /
    Document Length:        0 bytes
    
    Concurrency Level:      1
    Time taken for tests:   0.264 seconds
    Complete requests:      100
    Failed requests:        50
       (Connect: 0, Receive: 0, Length: 80, Exceptions: 0)
    Non-2xx responses:      50
    Total transferred:      32350 bytes
    HTML transferred:       2550 bytes
    Requests per second:    882.39 [#/sec] (mean)
    Time per request:       1.283 [ms] (mean)
    Time per request:       1.283 [ms] (mean, across all concurrent requests)
    Transfer rate:          278.83 [Kbytes/sec] received
    
    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    1   0.1      0      1
    Processing:     0    1   2.1      1      21
    Waiting:        0    1   2.1      1      21
    Total:          0    1   2.1      1      22
    
    Percentage of the requests served within a certain time (ms)
      50%      1
      66%      1
      75%      1
      80%      8
      90%      13
      95%      17
      98%      21
      99%      22
     100%      22 (longest request)
    ```
- 禁止curl访问
## 实验遇到的问题
#### 问题一：安装VeryNginx时出错
- No gmake nor make found in PATH
    ```bash
    sudo apt install make
    ```
#### 问题二：安装依赖包出错
- Package 'libssl1.0-dev' has no installation candidate
	```bash
	apt-cache search libssl # 发现只有libssl-dev包
    
  sudo apt install libssl-dev # 所以下载libssl-dev
	```
#### 问题三：权限不足
- cannot create directory ‘/opt/verynginx’: Permission denied
    ```bash
    sudo python3 install.py install # 要加上sudo
    ```
#### 问题四：无法保存nginx.conf的修改
​	在```vim```前面加上```sudo```，并在退出的时候将```:!q```改成```:wq```
#### 问题五：无法连接192.168.56.103
- nginx: [emerg] bind() to 192.168.93.4:80 failed (98: Address already in use)
  
  **1. 这是80端口被占用，所以要查看哪个进程占用了，然后结束这个进程**
  
  ```bash
  netstat -ntpl # 查看进程情况
  
  ps -ef | grep nginx # 发现时进程号698占用了
  
  sudo kill -QUIT 698 # 结束这个进程
  
  sudo /opt/verynginx/openresty/nginx/sbin/nginx # 重启nginx
  ```
  
    **2. 或者将端口改成81**，然后输入```192.168.56.103:81```连接
#### 问题六：wordpress路径问题, 出现Sorry, but I can’t write the wp-config.php file.
```bash
# 为wordpress 添加读写权限
sudo chown -R www-data:www-data /var/www/html/wp.sec.cuc.edu.cn/wordpress
```
#### 问题七：80端口被占用
[解决80端口被占用的情况](https://www.cnblogs.com/yuuje/p/12395339.html)
#### 问题八：无法通过域名访问
![](img/无法访问wp.png)
- 因为没有配置端口的监听
  * 添加matcher
    ![](img/match_wp.png)
  * 添加Up Stream
    ![](img/us.png)
  * 添加Proxy Pass
    ![](img/pp.png)
#### 问题九：执行```ab -n 100 http://wp.sec.cuc.edu.cn/```报错
- Benchmarking wp.sec.cuc.edu.cn (be patient)...apr_sockaddr_info_get() for wp.sec.cuc.edu.cn: Name or service not known (670002)
  ```bash
  # 编辑虚拟机的hosts文件
  sudo vim /etc/hosts
  
  # 加上以下内容
  192.168.56.103 vn.sec.cuc.edu.cn
  192.168.56.103 dvwa.sec.cuc.edu.cn
  192.168.56.103 wp.sec.cuc.edu.cn
  ```
## 参考资料
[安装VeryNginx的指南](https://github.com/alexazhou/VeryNginx/blob/master/readme_zh.md)
[解决nginx80端口被占用的情况](https://blog.csdn.net/yusiguyuan/article/details/20565337)
[linux-2020-cuc-Lynn](https://github.com/CUCCS/linux-2020-cuc-Lynn/blob/chap0x05/chap0x05/chap0x05%E5%AE%9E%E9%AA%8C%E6%8A%A5%E5%91%8A.md)
[删除软连接](https://blog.csdn.net/wackycrazy/article/details/46639785)