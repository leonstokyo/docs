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

