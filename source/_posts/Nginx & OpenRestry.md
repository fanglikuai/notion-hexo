---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWJB6M3S%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJIMEYCIQD2EqmzMyOv9oq%2FDACajNnPlyx5ZsNeU3bJ%2BrLQPku7xwIhAOWWcK9u95rTOLrk1Q2%2FVXQoTkbdegfhkXipz5Wt0vhXKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igya2rFdZ2oUd9R%2BG84q3AOHUdPBSKjoMDfXenMHzgPoQTXbzl0KqHbGL2TNt6D6rld%2FoOTlWVyJAn3dgo%2BFB0WWy04VXiniLdT3KNHof%2FDOWqq1o9KQyYdT4vbFSE5pcS2OKbw2RDcVrFRP8vUM1LstYDHdxWrTqupnLlvcC%2FbnZyELTTvDrPuc%2Fd%2FaPh5w%2FSZQTG192BuEfRkwfUViGttvzpX7KyIlsyYNt7PGdWpEHAmhIN4Q7s4950zKsgVgAWkYN6ReFkzvvZvNXOpxHKe1AnwK7uzu3tajNHQf9UvwHw%2BbOfHC0PtXAEosuXovlioX4WOdPxBKJFDCSCKkJMVQR32Me%2B0EHcgOm5bgiMYKy5sq9EIQuR2%2Bm%2FFoYyalsKXUBgOouAbRTPqeNerRDqApAzTXuodioBGOODeuQi2GsicgjU0K%2Bo40nKZLPSG%2BA96MSZn4pvOdtXe7E8f57SEi3ornsKh715LJIAqx%2FUPWuEai2WsvNukOmR77%2BmRDNHRx6MCVtFKCNSo73M6XQeOVOY0L%2FUv40Fdn%2BHo9IavJ5ay2IlXEEnUzFEE7sG3gPcG5t%2F7v%2FrWlQXO3X5i3P%2B2axQW%2F6Za6CqANP9iOZ9lVuvPr50zwOP2nTZEHub3Ile6qOqfDK7NayAC2jTCbk5DIBjqkAQs727pKWA9OmlE8UFv4MTSDq36lQ6t2hMJimqs%2BMaeUBj1Aa%2BbCPZ%2FXiON8Q3qsUfeZ%2FCRsWzEMqStnrPXOSrjJEwP8kxLyuArwkPUDnmAQ9PKRbuYxc6ogeDdIcnlNZKQhL4NS7M8PKV9W5MX73hEuBHNa8EdSfWsJNhtFsajf3%2F%2BfYxaAL8kvJWJhnHbTsecwCqs1m3jW%2B1ko4NXLLp5QnKbc&X-Amz-Signature=ee26dbc7844b45206280a361c0a9f6bb94002ced2c9bc8cd8e9c1622148d0998&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
