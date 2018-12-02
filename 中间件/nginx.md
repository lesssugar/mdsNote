# 环境搭建

```bash
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel	//安装gcc的基础库
yum install -y pcre pcre-devel
yum install -y zlib zlib-devel
yum install -y openssl openssl-devel
yum -y install wget httpd-tool vim	
```

## 关闭Iptables

```bash
iptables -L	//查看有无对应规则
iptables -F	//关闭规则
iptables -t nat -F	//关闭
systemctl stop firewalld.service
ststemctl disable firewalld.service	//开机禁用防火墙
vim /etc/systemconfig/selinux	删除enforcing,改成disabled
```

# 创建目录

```bash
opt下创建
app		//放项目
backup	//配置文件备份
download//下载的软件包
logs	//日志
work	//工作区
```

# 介绍

## 优势

```yaml
IO多路复用opoll
让一个socket多路复用完成多个IO流，多个描述符的i/o操作都能在一个线程内并发交替第顺序完成，这就叫I/O多路复用，这里的复用为同一个线程 
原来使用的select模型，由socket多线程监视每一个请求文件是否准备好，比较浪费资源
轻量级
只保留了和http有关的功能模块，其他功能模块化处理，可以很好的进行二次开发
CPU亲和
对于多核处理器，可以将不同的工作分配到不同的CPU上，可以获得更好的性功能，减少CPU的cache miss
sendfile
可以很好的处理静态文件，正常的文件需要经过用户控件，逻辑层，内核空间，传递给用户，但静态文件一般不刷要经过逻辑的处理，nginx就是用该种
```

# 快速搭建

```bash
http://nginx.org/
右侧download
最底下stable version	//稳定版本
复制	[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/OS/OSRELEASE/$basearch/
gpgcheck=0
enabled=1
vim /etc/yum.repos.d/nginx.repo	//新建一个nginx的yum源
将上面的复制进去，修改/OS/OSRELEASE/为 /centos/7/
yun list|grep nginx	//就可以查看可安装的nginx版本
yum install nginx  //即可安装成功
```

# 安装目录

```bash
rpm -ql nginx		//对于npm来说安装的都是一个一个的rpm包，用该命令可以查看全部文件对应的位置
/etc/logrotate.d/nginx			//对日志按周期进行处理的功能命而已按天

/etc/nginx
/etc/nginx/nginx.conf		//主要配置文件
/etc/nginx/conf.d
/etc/nginx/conf.d/default.conf

/etc/nginx/fastcgi_params	//cgi相关配置,cgi通用网关接口，定义了使用浏览器向服务器请求数据的标准
/etc/nginx/scgi_params
/etc/nginx/uwsgi_params

/etc/nginx/koi-utf			//编码转化的映射文件
/etc/nginx/koi-win
/etc/nginx/win-utf

/etc/nginx/mime.types		//设置http协议的Content-Type与扩展名对应关系

/etc/sysconfig/nginx		//用于配置出系统守护进程管理器管理方式
/etc/sysconfig/nginx-debug
/usr/lib/systemd/system/nginx-debug.service
/usr/lib/systemd/system/nginx.service

/etc/nginx/modules			//nginx模块目录
/usr/lib64/nginx/modules

/usr/sbin/nginx				//服务的启用命令
/usr/sbin/nginx-debug

/usr/share/doc/nginx-1.14.1	 //nginx的手册和帮助文档
/usr/share/doc/nginx-1.14.1/COPYRIGHT
/usr/share/man/man8/nginx.8.gz

/var/cache/nginx		//nginx的缓存目录

/var/log/nginx			//nginx日志

/usr/lib64/nginx
/usr/libexec/initscripts/legacy-actions/nginx
/usr/libexec/initscripts/legacy-actions/nginx/check-reload
/usr/libexec/initscripts/legacy-actions/nginx/upgrade
/usr/share/nginx
/usr/share/nginx/html
/usr/share/nginx/html/50x.html
/usr/share/nginx/html/index.html
```

## 安装编译参数

```bash
nginx -V
--prefix=/etc/nginx 				//安装目录和路径
--sbin-path=/usr/sbin/nginx 
--modules-path=/usr/lib64/nginx/modules 
--conf-path=/etc/nginx/nginx.conf 
--error-log-path=/var/log/nginx/error.log 
--http-log-path=/var/log/nginx/access.log 
--pid-path=/var/run/nginx.pid 
--lock-path=/var/run/nginx.lock 

--http-client-body-temp-path=/var/cache/nginx/client_temp 	//对应模块的临时文件
--http-proxy-temp-path=/var/cache/nginx/proxy_temp 
--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp 
--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp 
--http-scgi-temp-path=/var/cache/nginx/scgi_temp 

--user=nginx 		//设定用户和组用户
--group=nginx 	

--with-compat 
--with-file-aio 
--with-threads 
--with-http_addition_module 
--with-http_auth_request_module 
--with-http_dav_module 
--with-http_flv_module 
--with-http_gunzip_module 
--with-http_gzip_static_module 
--with-http_mp4_module 
--with-http_random_index_module 
--with-http_realip_module 
--with-http_secure_link_module 
--with-http_slice_module 
--with-http_ssl_module 
--with-http_stub_status_module 
--with-http_sub_module 
--with-http_v2_module 
--with-mail 
--with-mail_ssl_module 
--with-stream 
--with-stream_realip_module 
--with-stream_ssl_module 
--with-stream_ssl_preread_module 
--with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' 
--with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie'
```

## 配置文件

```bash
cd /etc/nginx
vim nginx.conf

user  nginx;		//设置nginx的系统使用用户
worker_processes  1;//工作进程数，一般和cpu保持一致

error_log  /var/log/nginx/error.log warn;	//错误日志
pid        /var/run/nginx.pid;				//进程启动时的pid记录在这里面


events {
    worker_connections  1024;			//每个进程允许的最大连接数
    use								//工作进程数，使用的内核模型
}


http {
    include       /etc/nginx/mime.types; 	//
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '	//日志类型
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;		//支持sendfile功能
    #tcp_nopush     on;

    keepalive_timeout  65;	//断开时间	65s

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;	//引用其他的配置文件
}
```

```bash
default.conf文件

server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
修改了配置文件后需要重启服务
systemctl restart nginx.service
ststemctl rload nginx.service
```

# 模块

```bash
--with-http_stub_status_module
nginx当前处理连接的状态
上面的配置文件中添加
location /mystatus {
    stub_status;
}
nginx -tc /etc/nginx/nginx.conf
nginx -s reload
访问mystatus会展示nginx的信息，连接数
第二项三个参数	1.nginx处理的握手次数 2.nginx处理的连接数 3.nginx所处理的总的请求数，前俩个相等说明没有丢失 
```

```bash
--with-http_random_index_module
随机生成首页，一般很少用到，除非我们想给用户不同的首页的感觉
location / {
 	  root /user/share/nginx/htnl
      #index index.htmk index htm
      random_index on;
}
```

```bash
limit_conn_moudle		//连接频率限制
limit_conn_zone key zone=name:size;	
context:http;
key是要限制的东西比如客户端的ip就写内置变量客户端的ip，name申请空间的名称，size:1m,2m110m
limit_conn zone number	//zone写上面申请的空间的名字，并发的数量限制

limit_req_moudle 	//请求频率的限制
limit_req_zone key zone=name:size rate=rate;
limit_req zone=name [burst=number][nodelay]	//zone写上面申请的空间的名字，并发的数量限制
```

