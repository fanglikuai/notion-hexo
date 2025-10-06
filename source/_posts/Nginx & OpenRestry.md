---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBZ7TWHQ%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAiQ%2FLy7uUU4wpI1EQewzI%2BdbAs8gwBX1wMVWQbiB2rqAiEAnIpSH5NjEZ3h9x4mziPSpsOBgU4N4f7gv76kpUBXH2UqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLGeS4ZqCI1FEqIkYircAycOWIjy895sIUykMQ8sd4VmyA2ysL5ab8bJLT6OpNPnSk4Dt%2FQiKNaNcu0XIaPb4pfhD4oLge6CrD%2Fr2ZI0IvWPyBnLgvB4z7i8fcTVkRO%2FKUqOdmzmAzsLWRE3qRozjEb2CHJdDqaRJtOGQqTzwJ07o2MWasoyJezbsd1moVbGCzg3%2F5IrttTnqHbEFVUqT%2FUmGo%2FCbHqkbcViAZB4Ka5iokVkkU0vVI0e6aDbCPpHvVFWNTkbSXUpDBG%2BLtpu7XPOk2uL%2BKQQxnrsSk6O62M8eRcY1zyy4UkxNepP3LtB0L9V5rR3Rm3G3Za%2Ft9VqfFMM7Zu96xW5Mh5h3DDMPvr4IWHRgm16B4YksUclwbfaOkKGaHAjETiraRfg58hrY%2F6SgwUJFQffOEpGBia3lgS8brRjAspls7jfzJqpkvWegrTXxbvBpOFQKIdNQB%2FeOyjS04BVVjJLJfevQHQwZKCeYCwo1MQpdNsklxc61dv9rnQslcqLcvEEY6Ax%2FvxBcw9NwyowwzJfw0UaDk7za3XLTPuggm7g4%2BMNbDjn%2FO33H2UXABZkN9AkhEroPx%2F1dxU%2BSHtvJW7iXAnIpLt9Ddxw3%2B%2B%2B0A%2Funq99J3Phf%2BeiYXSV8MubnrxrkcLFMM3%2Bi8cGOqUB1jX8s7RUlKKZrd3yo5WYbWvkci9sk%2FnwONijJw88lbnkFBm9cFYTONUpHTjwBQ5IhBjVIbeee2dmCxdrS0cr6vA3EbWCpFNI5FCaUBbLwCGutN9ENb6QneOKzf%2FMg3KzQdpHK3Rcf3ldhN05wsXtDTCnK0izUzVHC%2BIXc5ZhEefdtvM%2FitWatyqtShLPHeiq814Ok8BTjJnFPnSYHvEBorR4AMF2&X-Amz-Signature=a1b1841931dbd76ac5845b6c1a40a607f49ccbcbcfe0b4413796d7526aedb990&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
index_img: /images/681caddd167c86081c93eb4da2dc581a.png
banner_img: /images/681caddd167c86081c93eb4da2dc581a.png
---

# 基本概念


**Nginx (engine x)** 是一款轻量级的 Web 服务器 、反向代理服务器及电子邮件（IMAP/POP3）代理服务器。


**反向代理与正向代理的区别：**


正向代理：在用户这一端，vpn


反向代理：在服务器端，nginx

> 拓展：
>
> 堡垒机：统一的运维入门，带权限认证
>
>

基本使用：


```bash
nginx -s stop
#快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。nginx -s quit
#平稳关闭Nginx，保存相关信息，有安排的结束web服务。nginx -s reload
#因改变了Nginx相关配置，需要重新加载配置而重载。nginx -s reopen
#重新打开日志文件。nginx -c filename
#为 Nginx 指定一个配置文件，来代替缺省的。nginx -t
#不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。nginx -v
#显示 nginx 的版本。nginx -V
#显示 nginx 的版本，编译器版本和配置参数。
```


# 实战


反向代理域名的tomcat


```plain text
upstream zp_server1{
  server 127.0.0.1:8080;
  # 写要代理的地方
}
server {
  listen       80;
  server_name  www.helloworld.com; #从哪里来的域名

  #charset koi8-r;

  #access_log  logs/host.access.log  main;

  location / {
    #  root   html;
    # index  index.html index.htm;
    proxy_pass http://zp_server1;
    #进行代理
  }
```


## 跨域问题

1. 在 Nginx 的`server` 或`location`块中添加以下头部：

```plain text
location / {
  add_header 'Access-Control-Allow-Origin' '*';
  add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
  add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
  add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';

  # 处理预检请求 OPTIONS
  if ($request_method = 'OPTIONS') {
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
    add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
    add_header 'Access-Control-Max-Age' 1728000;
    add_header 'Content-Type' 'text/plain; charset=utf-8';
    add_header 'Content-Length' 0;
    return 204;
  }

  # 其他请求正常处理
  ...
}
```

1. 指定的域名可以跨域
