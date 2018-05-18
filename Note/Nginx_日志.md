# 日志类型
error.log 错误状态

access_log 访问状态

# 配置语法

log_format

```
Syntax:log_format name [escape=default|json] string...;
Default:log_format combined "..."; 
Context:http
```

# 配置文件

vim /etc/nginx/nginx.conf

存储路径 日志级别

```
error_log  /var/log/nginx/error.log warn;

```


```
http{
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
}
```

tail -n 200 /var/log/nginx/access.log

# 日志变量

HTTP请求变量 - arg_PARAMETER、http_HEADER、sent_http_HEADER

'$http_user_agent'

内置变量 - Nginx内置的

自定义变量-自己定义


nginx -t -c config_file_path 测试指定路径配置文件是否正确
nginx -s reload -c config_file_path 重新加载配置文件


