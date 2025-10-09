---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGD3PEVJ%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJHMEUCIDZ5dyEChOiCpzWLa4t4BKNyi%2FnSMHkgBtnslwJP%2BvZJAiEA5sJ9c7y5Tgk%2Br%2FW3%2Ffksouo%2FqW32580Lpy0ntujvFBUqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEn1xJylnbuQpt9eHCrcA01sWkfH646z0G03o8XQy8UOF1gLX6Sc7p19zZhQMEV0qf%2Fkk5qzGZ%2BD47OlWGQdYL9zZep8o%2BrOCz3yO5LomCFQh2bkMbMfm%2FsRAYmCGaL1KKG%2Bl6l6UFj0NJWZJQBcvcDCqfkOWT6ksOXX710RwmCUQCvgozoG0%2Bf6j2YKgXb2JnuCxT4oIj9LsMaaJvH7gdQrMcVOM5KMUOiAQ4ABEZys3UbQLDnnihWe5mq5TWcWmakKdgm2DZPqgbB68M07sVEmq4AhvlfDjAA5Qva3XDQ%2FWBgY%2F%2F6fCt6RgkCnuueMkBQo65xCrvyAjtZVYjV7MrqrFhVoyE%2BZ2fp%2B97y0ZDmR6%2BYWZXqdpTRuB%2Bw8J%2BT5LKsDlSOPkUxHKb25RC93MKvhNkoNTNYtGXeJoECjtmYV0aJOhmaN3XynWGFYcynQQ7cFSr23%2BXdOaW2BWbE1vteBscX447aOHaqH2D6egJ6PW4DIfE4ig4fc3qJVFL7hQD%2F6kigi9o3WE9WhuiLTzl%2BuvRQ6ipl2sCee271JlCQVZOwNiFL5P9L%2BXHNmgPn0Ko711WfnOV6qlr42zqNVUaIfcrpFjJXCPmctYvr%2FvZetzpN74p38N6%2BFSc%2BevKgPw4%2FelF%2FqhWJ4UhRHMIHRnMcGOqUBEYv%2BmjJwKu%2BvHH7872cSpOPaegED08Mhjve7sY7HS%2FdDZ9K1noz%2Bk%2BsMJzm554Ww4WQEOZq%2BnYupcN1IX%2BGERHpvWyxVSNimWt3B%2Fb%2Bxj4NeF3dZI1gj0i%2BIvEM0SvByIaSexv3%2BRGi1JgXcsPlkZdVOR0XVoyXr75pYgSM15GyIbjgSyAO8mbL58uGP9y%2FPmGDs2rTWm4VFBO6hsodDLHTw717b&X-Amz-Signature=ea2c55f55f8c5ea8a56e19f84d8a4ac4ea47fa39cb08b1d12955fb42842ba587&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
