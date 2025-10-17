---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YYB2JFZ%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T010052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDUvNInfEHOZamrFD%2FN%2BjlqYy9zC5kqYpx0L0%2B1uuuZfAiA482YKX9iC8%2FD6YaXH8DoyL47Dc0N6AILWhjkoRrzKgSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMH5QlWlDSh0eU3GkPKtwDZqvFKZCDVbdMEdvFA1%2FrHDvPbgMKcx4FglWayr%2BrCG5hwxElZqorzkHBH5x2spVkZNuTgk9i8OiFSPXV1QGGHgPQ89cke2HXmBxF%2BEAqvTrVp8EGN9poO%2Bfly%2Bu24YmVnA7mqBe2EGMTos1NEUY1agtWujt3Dz6QMcPNBuEk2VPyxmDpcGmmAvwZCTvcsL1Af2he7tj5XdAjFyV5PV4J88cfRlqRHCtdzGc8TlgjiaPu21a0D7WQnly1r2ntEpe8p14OR6wD5ej39l8u0%2Bmv93fDyCz5xMo8gpwmJUM08AWaaxqtMHqyF8KwIB9kltZ2INZ8Z%2B%2FmtbEyec%2F0Y5nOdr77qZP7tRpqDRvJbGISuFMX3QkDjXN5KgWaUBtdx0mRzbvFDevAQFAXDl%2F9baiC1omUgrJXAMkjXZpdBb9ajOAiFOypDT9fjNt4hnEnkN7QzPvoM078t1fr5I6BJo%2B0ENtOz3ipsYtDwwMtyRz%2BmEbj%2FFFHy6Rku9jla%2BQ9I%2B61sRYel01pt%2FfJ8VAFVbL9O7pcqffTotUGmZXsw9O03Cn23S%2Fm2zGgK7HXbSCJVJDos%2FAURlzE0hGiGd4ABo5mfLOqQwWFevAPXNrzV%2FbqsTPH9K07AO4LF5%2Bx3Ycw7JzGxwY6pgGusz4TJSFpLYzccdiNoazb07%2Bz0D6L%2FD48Y5yls5%2BojSb0mXd5vhYi65P%2BAZIWhDdZLklorsBPuOh2iXEpihluqtmkjR7eq4Ow27XaKpnCiSzYjdiVB6MW1X7sbk3eytYaS%2F80UxIn7QqB8uR15P%2BCVh5yCK7O3%2Fw61SotCfezpHrh52xYFxR3OjU%2B6QspQFraV7BPhEezKRBxYvtKBgIT5MjyUfhB&X-Amz-Signature=bef254e1da67944c58582c54fedfae4ad8c09d93ce23d6077d5ab1732c6f4147&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
