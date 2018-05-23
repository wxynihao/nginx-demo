# 1 静态资源类型

非服务器动态运行生成的文件
 
# 2 CDN场景
 
## 2.1 配置语法

* 文件读取

```
Syntax:sendfile on | off; 
Default:sendfile off; 
Context:http,server,location,if in location
```

--with-file-aio 异步文件读取

* tcp_nopush

```
Syntax:tcp_nopush on | off; 
Default:tcp_nopush off; 
Context:http,server,location
```

作用：sendfile开启的情况下，提高网络包的传输效率

* tcp_nodelay

```
Syntax:tcp nodelay on | off;
Default:tcp_nodelay on; 
Context:http,server,location
```

作用：keepalive连接下，提高网络包的传输实时性

* 压缩

```
Syntax:gzip on | off;
Default:gzip off; 
Context:http,server,location,if in location
```

作用：压缩传输，由浏览器负责解压

```
Syntax:gzip_comp_level level; 
Default:gzip_comp_level 1; 
Context:http,server,location
```

作用：指定压缩比

```
Syntax: gzip_http_version 1.0|1.1; 
Default: gzip_http_version 1.1; 
Context: http, server, location.
```

作用：指定压缩版本

## 2.2 压缩方式

http_gzip_static_module - 预读gzip功能

http_gunzip_module - 应用支持gunzip的压缩方式（不支持gzip时，很少使用）
 
## 2.3 场景演示

```bash

    location ~ .*\.(jpg|gif|png)$ {
        gzip on;
        gzip_http_version 1.1;
        gzip_comp_level 2;
        gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
 
        valid_referers none blocked 116.62.103.228 jeson.imoocc.com ~wei\.png;
        if ($invalid_referer) {
            return 403;
        }
        root  /opt/app/code/images;
    }

    location ~ .*\.(txt|xml)$ {
        gzip on;
        gzip_http_version 1.1;
        gzip_comp_level 1;
        gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
        root  /opt/app/code/doc;
    }

    location ~ .*\.(htm|html)$ {
        expires 24h;
        root  /opt/app/code;
    }

    location ~ ^/download {
        gzip_static on;
        tcp_nopush on;
        root /opt/app/code;
    }
    
```

gzip_static是指在使用之前先压缩

```bash
gzip src
```

name.xxx 会变成 name.xxx.gz

# 3 浏览器缓存场景

## 3.1 原理


## 3.1 演示
 
# 4 跨站访问场景
 
## 4.1 配置
 
# 5 防盗链
 