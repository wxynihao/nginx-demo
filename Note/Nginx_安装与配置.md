# 1 安装

## 1.1 添加yum仓库

1. 添加yum repository，创建名为 **/etc/yum.repos.d/nginx.repo** 的文件

2. 操作系统版本查看命令为：`lsb_release -a`

3. 添加以下内容，修改`OS`为`centos`等，`OSRELEASE`修改为版本号`7`等。

```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/OS/OSRELEASE/$basearch/
gpgcheck=0
enabled=1
``` 
## 1.2 查看仓库

```
 yum list |grep nginx
```

## 1.3 安装

```
yum install nginx
```

## 1.4 检查

查看版本

```
nginx -v
```

# 2 目录

## 2.1 查看目录

列出全部软件全部安装文件及其路径。

```
rpm -ql nginx
```

## 2.2 目录详解

路径|类型|作用|
---|---|---
/etc/logrotate.d/nginx | 配置文件 | Nginx日志轮转，用于logrotate服务的日志切割
/etc/nginx <br/> /etc/nginx/nginx.conf <br/> /etc/nginx/conf.d <br/> /etc/nginx/confd/default.conf | 目录、配置文件 | Nginx主配置文件
/etc/nginx/fastcgi_params <br/> /etc/nginx/uwsgi_params <br/> /etc/nginx/scgi_params | 配置文件 | cgi配置相关，fastcgi配置
/etc/nginx/koi-utf<br/>/etc/nginx/koi-win<br/>/etc/nginw/win-utf | 配置文件 | 编码转换映射转化文件
/etc/nginx/mime.types | 配置文件 | 设置http协议的Content-Type与扩展名对应关系
/usr/lib/systemd/system/nginx-debug.service <br/> /usr/lib/systemd/system/nginx.service <br/> /etc/sysconfig/nginx <br/> /etc/sysconfig/nginx-debug | 配置文件 | 用于配置出系统守护进程管理器管理方式
/usr/lib64/nginx/modules <br/> /etc/nginx/modules | 目录 | Nginx模块目录
/usr/sbin/nginx <br/> /usr/sbin/nginx-debug | 命令 | Nginx服务的启动管理的终端命令
/usr/share/doc/nginx-1.12.0 <br/> /usr/share/doc/nginx-1.12.0/COPYRIGHT <br/> /usr/share/man/man8/nginx.8.gz | 文件、目录 | Nginx的手册和帮助文件
/var/cache/nqinx | 目录 | Nginx的缓存目录
/var/log/nginx | 目录 | Nginx的日志目录

# 3 编译参数

## 3.1 查看编译参数

```
nginx -V
```

## 3.2 参数详解

编译选项 | 作用
--- | ---
--prefix=/etc/nginx <br/> --sbin-path=/usr/sbin/nginx <br/> --modules-path=/usr/lib64/nginx/modules <br/> --conf-path=/etc/nginx/nginx.conf <br/> --error-log-path=/var/log/nginx/error.log <br/> --http-log-path=/var/log/nginx/access.log <br/> --pid-path=/var/run/nginx.pid <br/> --lock-path=/var/run/nginx.lock | 安装目的目录或路径
--http-client-body-temp-path=/var/cache/nginx/client_temp <br/> --http-proxy-temp-path=/var/cache/nginx/proxy_temp <br/> --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp <br/> --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp <br/> --http-scgi-temp-path=/var/cache/nginx/scgi_temp <br/> | 执行对应模块时，Nginx所保留的临时性文件
--user=nginx <br/> --group=nginx <br/> | 设定Nginx进程启动的用户和组用户
--with-cc-opt=parameters | 设置额外的参数将被添加到CFLAGS变量
--with-ld-opt=parameters | 设置附加的参数，链接系统库

# 4 配置

## 4.1 查看配置
```
cd /etc/nginx
vim nginx.conf
```

## 4.2 配置文件

* nginx.config

主要配置文件，include了其他配置文件

```
include /etc/nginx/conf.d/*.conf;
```

* default.conf

```
vim /etc/nginx/conf.d/default.conf
```


## 4.3 配置字段

字段 | 作用
--- | ---
user | 设置nginx服务的系统使用用户
worker_processes | 工作进程数(一般与CPU核心数量一致)
error_log | nginx的错误日志
pid | nginx服务启动时候pid
events.worker_connections | 每个进程允许最大连接数
events.use | 工作进程数

## 4.4 配置语法

### 4.4.1 nginx.config
* Linux
```

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}

```
* Windows
```
http {
    server {
            listen       80;
            server_name  localhost;
    
            location / {
                root   html;
                index  index.html index.htm;
            }
    
            error_page   500 502 503 504  /50x.html;
            location = /50x.html {
                root   html;
            }
         }
    }
```

### 4.4.2 default.conf

```

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

```

修改后，启动或重启即可生效。
```
 systemctl start  nginx.service
 systemctl restart  nginx.service
 systemctl reload  nginx.service
```