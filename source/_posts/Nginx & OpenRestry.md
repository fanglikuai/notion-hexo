---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWW6V5JM%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T090054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDpUOCIDklE9vA3FYSCiDzV8gE%2BGI%2FErRfdBPKMKCGtzgIgFUjOeBXpR8sOS8Lk3%2Beo%2Fkts%2F9Q8aAxb8UwWRbB78J0q%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDLHkC56yH63mu2y74CrcA7x3ldH8iD4YmT0da1l1n72fOXFviRQVB5fX3UEnlGTAnfQkS%2FvMc%2BFznklvZ14xo9Yjr7CWr8KYtPu4jToX2Jf8K%2FaNG23R7D7FseK7Q6wONPXaCFt9xOJCGxRqiGzjbc3DfMN2iu7hXn5qHni6lWcVd8cHkJPEU7Dfzuq4SOUUvI2ZnthmGWjUzdqDrH6QigqyyHJVDiwN3%2BUnuhBw%2FfOJCciMvgzSKlyjZb8Qnze%2B5LYb7af7pRJ0qKFOTTUgWI3d5vEaB%2Bdvfas7%2BqRwtNxGL1M87BcMqrIcfofWrbE0XSx%2Bk0vvJMOD3b7%2FE%2BsWvAx0B600p22Co58BVVbixrHfBV%2BKtZYLG5I22xnKqCXFw4M96jTdIh22GnR5K3su3qqjg8kFYXQw5YFHWMaU0%2BVIuqITFdbm2uSFGbODoee84MWuuNdu3WTd1Hs7w1odQEOerMog8SeOgj8owcso2ZHm6sAnlkmqpC86F7PYffftGJT0CWDQIXJSzhqOUXtdfM1ug8N%2F7UXkzdyv7UlpEZiCj4JM9zdFlnMy6oixy4RrsP19koaoIqXsn7yvqjxRo%2FXaawIPBjO2xBg423ZCzTiQvj08qPrRPqffviFAZhr76wANK%2BUuSwqwHmOLMLfq8ccGOqUBwmAmy6NQ1QK3oCtmmgRvrczRjutISnBwqr%2Fpc0zR81VV8YlCk9Ywb9NsLcAYY5PNorkxiKPsHnnue8HTTZboUFyhOWkiHbe4HLdeYiF1ErKSK76%2FNu7VO1Q39nh4pvSHcgjA5LdLqzi4HwZarXfbrsMZi0Fg16oFUcPzW8lCmoKy4CsF5dmA9AEguUwIsr9jkjjwE7wvfRE2pRHKpfn7AZSsxBcB&X-Amz-Signature=9f4e9ae7cdc66c160f633f1a62acb54f7c5b2a005435f8c7f78dd2a3ad6edc57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
