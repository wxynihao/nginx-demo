# 概述

## 简介

### 中间件

### Nginx

开源且高性能、可靠的HTTP中间件、代理服务。

### 常见的HTTP服务

HTTPD Apache

IIS MicroSoft

GWS Google

## Nginx特性

* IO多路复用epoll

什么是IO复用

什么是IO多路复用
多个描述符的I/O操作都能在一个线程内并发交替地顺序完成，这就叫I/O多路复用，
这里的“复用”指的是复用同一个线程。

什么是epoll
IO多路复用的实现方式select、poll、epoll

select模型
图

epoll模型
1）每当FD就绪，采用系统的回调函数之间将fd放入，效率更高。
2）最大连接无限制

* 轻量级
 功能模块少、代码模块化


* CPU亲和（affinity）
一、什么是CPU亲和
是一种把CPU核心和Nginx工作进程绑定方式，
把每个worker进程固定在一个cpu上执行，
减少切换cpu的cache miss，获得更好的性能。
一、为什么需要CPU亲和

* sendfile
图




