# Nginx

## Nginx介绍

* 为什么要学习Nginx？
  * 客户端到底要将请求发送给哪台服务器
  * 如果所有客户端的请求都发送给了服务器1
  * 客户端发送的请求可能是申请动态资源的 也有申请静态资源的

* 在服务器搭建集群后，用Nginx做反向代理服务器

![image-20210105185849351](TyporaPics/image-20210105185849351.png)

* Nginx特点
  * 稳定性强，7*24无间断运行
  * 提供丰富配置示例
  * 占用内存小，并发能力强

## Nginx的配置文件

首先是/etc/nginx目录下的nginx.conf：

```nginx
#cat /etc/nginx/nginx.conf

#全局块
user  nginx;
worker_processes  1;								#worker_processes数值越大，Nginx并发能力越强
error_log  /var/log/nginx/error.log warn;			#错误日志存放位置
pid        /var/run/nginx.pid;

#events块
events {
    worker_connections  1024;						#worker_connections数值越大，Nginx并发能力越强
}

#http块
http {
    include       /etc/nginx/mime.types;			#引入了/etc/nginx/mime.types，mime.types中存放着大量媒体类型
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    keepalive_timeout  65;
    include /etc/nginx/conf.d/*.conf;				#引入了conf.d目录下所有以.conf为结尾的配置文件
}
```

默认conf.d目录下只有一个default.conf：

```nginx
#http块下的server块
server {
    listen       80;								#Nginx监听的端口号
    listen  [::]:80;
    server_name  localhost;
    
#http块下的server块下的location块
    location / {
        root   /usr/share/nginx/html;				#将接收到的请求根据/usr/share/nginx/html去查找静态资源
        index  index.html index.htm;				#默认去上述路径中找到index.html或index.htm
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

## 反向代理

### 正向代理与反向代理介绍

* 正向代理
  * 由客户端设立
  * 客户端明确代理服务器及目标服务器
  * 突破访问权限，提高访问速度，对目标服务器隐藏客户端ip
* 反向代理
  * 由服务端配置
  * 客户端不知道访问的哪一台服务器
  * 负载均衡，隐藏服务器真正ip

### 基于Nginx实现反向代理

修改http块内的server块即可（默认为/etc/nginx/conf.d/default.conf）

```nginx
server {
    listen       80;								#Nginx监听的端口号
    server_name  localhost;
    
    location / {
        proxy_pass http://www.jhyserver.top:8080/;	#基于反向代理访问的服务器
    }
}
```