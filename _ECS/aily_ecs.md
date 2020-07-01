# 阿里云服务器

> 服务器部署的相关记录（本机系统为CentOS 8）

## 安装Nginx
1.下载Nginx，到http://nginx.org/en/download.html 上选择合适的版本，我这里选择的是**nginx-1.19.0**

2.安装相关依赖
 * **gcc** -- linux下的编译器，它可以编译 C,C++,Ada,Object C和Java等语言
 
  `yum -y install gcc`
  
 * **pcre** -- 是一个perl库，包括perl兼容的正则表达式库，nginx的http模块使用pcre来解析正则表达式
 
   `yum install -y pcre pcre-devel`
                      
 * **zlib** -- 提供了很多种压缩和解压缩方式nginx使用zlib对http包的内容进行gzip
 
   `yum install -y zlib zlib-devel`
 
 * **openssl** -- web安全通信的基石
    
   `yum install -y openssl openssl-devel`  

3.源码编译

  * 创建安装目录
  
  `mkdir -p /usr/develop/nginx`
  
  * 将文件上传至服务器，我这里是在Mac上下载的
  
  `scp /Users/tyrell/Downloads/nginx-1.19.0.tar.gz root@47.114.47.44:/usr/develop/nginx`

  * 到安装目录下，解压，删除压缩文件
  
  `cd /usr/develop/nginx`
  `tar -zxvf nginx-1.19.0.tar.gz `
  `rm -rf nginx-1.19.0.tar.gz `
  
  * 进入目标文件夹相关命令
  
  `cd nginx-1.19.0`
  `./configure`
 
  反馈信息
  
   Configuration summary
      
   \+ using system PCRE library
      
   \+ OpenSSL library is not used
      
   \+ using system zlib library
     
   nginx path prefix: "/usr/local/nginx"
      
   nginx binary file: "/usr/local/nginx/sbin/nginx"
      
   nginx modules path: "/usr/local/nginx/modules"
      
   nginx configuration prefix: "/usr/local/nginx/conf"
      
   nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
      
   nginx pid file: "/usr/local/nginx/logs/nginx.pid"
  
   nginx error log file: "/usr/local/nginx/logs/error.log"
  
   nginx http access log file: "/usr/local/nginx/logs/access.log"
  
   nginx http client request body temporary files: "client_body_temp"
  
   nginx http proxy temporary files: "proxy_temp"
  
   nginx http fastcgi temporary files: "fastcgi_temp"
  
   nginx http uwsgi temporary files: "uwsgi_temp"
  
   nginx http scgi temporary files: "scgi_temp"
  
  * 编译安装
  
  `make && make install`

  * 启动nginx
  
  `cd  /usr/local/nginx/sbin`
  
  `./nginx` 启动
  
  `./nginx -s stop` 立即停止服务
  
  `./nginx -s quit` 从容停止服务
  
  `./nginx -s reload` 重启
  
  * 开启防火墙并开放80端口
  
  `systemctl start  firewalld`
  
  `firewall-cmd --zone=public --add-port=80/tcp --permanent`
  
  `firewall-cmd --reload` 重启
  
  * 阿里云需要配置安全组开放相关端口
  
  * 成功访问
  
   <img width="500px" src="_media/nginx-success.jpeg">

## 安装MySQL8.0
##### 使用最新的包管理器安装MySQL
`sudo dnf install @mysql`

##### 开启启动

安装完成后，运行以下命令来启动MySQL服务并使它在启动时自动启动：

`sudo systemctl enable --now mysqld`

要检查MySQL服务器是否正在运行，请输入：

`sudo systemctl status mysqld`

添加密码及安全设置

运行mysql_secure_installation脚本，该脚本执行一些与安全性相关的操作并设置MySQL根密码：

`sudo mysql_secure_installation`

步骤如下：

1. Would you like to setup VALIDATE PASSWORD component? Press y|Y for Yes, any other key for No: y(要求你配置VALIDATE PASSWORD component（验证密码组件）： 输入y ，回车进入该配置)

* There are three levels of password validation policy:

  LOW    Length >= 8

  MEDIUM Length >= 8, numeric, mixed case, and special characters

  STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

  Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0

  选择密码验证策略等级， 我这里选择0 （low），回车
  
* New password: 
    
  Re-enter new password:
  
  输入两次新密码
  
* Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y

  确认是否继续使用提供的密码？输入y ，回车

* Remove anonymous users? (Press y|Y for Yes, any other key for No) : y

  移除匿名用户？ 输入y ，回车
  
* Disallow root login remotely? (Press y|Y for Yes, any other key for No) : g

  不允许远程登录？输入其他字符
  
2. Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y

 移除test数据库？ 输入y ，回车    

3. Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y

 重新载入权限表？ 输入y ，回车
 
##### 配置远程登陆

如果需要设置root账户远程登陆，上一步骤中，**不允许root远程登陆？** 这一步需要设为n。

接下来本机登录MySQL，将root用户的host字段设为'%'，意为接受root所有IP地址的登录请求：

本机登录MySQL:

`mysql -uroot -p`

`password为刚才设置的密码`

接着继续执行mysql语句，将将root用户的host字段设为'%'：

`use mysql;`

`update user set host='%' where user='root';`

`flush privileges;`

设置完成后输入exit退出mysql，回到终端shell界面，接着开启系统防火墙的3306端口：

`sudo firewall-cmd --add-port=3306/tcp --permanent`

`sudo firewall-cmd --reload`

##### 关闭MySQL主机查询dns

MySQL会反向解析远程连接地址的dns记录，如果MySQL主机无法连接外网，则dns可能无法解析成功，导致第一次连接MySQL速度很慢，所以在配置中可以关闭该功能。

打开`/etc/my.cnf`文件，添加以下配置：

`[mysqld]`

`skip-name-resolve`

##### 重启服务

`sudo systemctl restart mysqld`

##### 其他

阿里云注意安全组的配置

