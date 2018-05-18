Nginx官方模块

第三方模块

编译参数 
包含编译模块
--with 部分

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
 
 # 安装编译参数详解
 编译选项 | 作用
 ---|---
  --with-http_stub_status_module | Nginx的客户端状态
  
## http_stub_status_module
```
Syntax:stub_status; 
Default:--
Context:server,location
```
 