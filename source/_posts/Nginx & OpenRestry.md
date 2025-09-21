---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHQWZTZO%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T220049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCy9YvNXzdLcrWf%2BENuC3IOUI1ilB5EvqFoNdokR%2Bw1GgIgE6OC%2F7CLMr4tNsQQ2nHGYeo%2BIaWkRS6h4JyYbJXHcHgq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDHavSz24sCh9zPVKyCrcA9kGV4Ud0qgglwzhPKUiuv4v%2FN%2FkFR442I5UCxphZoGCMGgo7BSsc53xU0JDsSUS7u1odJZ3WrzaHXx36J4D4upPdJrn7jgqO8sEIeUFhPxODJ04IklvkVCocguTy3EyOS3J9%2FhpOJ%2FmKgxTvBr%2F%2BLUD3wKj173KUw7n0D7tGdMi3XJVDOLYo1qhFxj0zREWYupOhEQE1SH1O9bf8plheGw6seArmtGqzwCLK0tgtv6N3RbEaYywh%2BxrCBgh%2FC3Y0G%2F651awJGO3HqB1OklCltK6DnhrORHWLlMk5fRt4lt41KhO6Q2dVmMGfihcaAKtI9KlkreQ%2FdlH%2FHHMDnW0nuLP7KG7UcqGY1Kg%2B5XBAUowKzchqxjhcBw56q0qWVqCrkfoUrN1j6%2FheoQi8zay1oFIIcwSgLz%2FLPIJEK%2BgX9MkPft%2B9lbUMBXgOK4Tr5nzndwd%2BmtBE00O5IyLHb5ovfBF9HIKZiKRNL59LX2u8ynKs%2BwmB6yEbKphHdBTpCjzhBZGWJUMPMOVdNA26WsGPuv024LxGXrt1s3Y9ARFma0WYelES9yqdjGA7M4YCCn6PW3c%2BJMGA1LYPZMPiuMG00AakDvVlyVE6ZEaLcVc0qvA%2F%2BjhZhNvWWqvm4tbMPTfwcYGOqUBcELv%2BuQWThIzzHeoi7kJp1lY7d1%2BpC6nZAvBgj0fCJRbWMlPzsEQj8SkmfWGsXCWTjK3b1aJcGgATEF8UK4No%2BJMdgGXw1log%2BZNuhnlYmmgMYZuJD0kwUl0vPLH5C5JBRVrB4Q8C5U%2FE0dniiKwXQciaj8eFY5SR2%2BZX3JtBkt%2BgRwwiVldVCoEQKp50BjPbNKqVn3W5YvF0dzb2ii02oYdeIDl&X-Amz-Signature=ef650e173ca174bd310023838fda69c0d4ab56ff12eccb9ffd4d8b809c0eebc3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
