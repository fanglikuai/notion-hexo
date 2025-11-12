---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Y3T6PYU%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJGMEQCIB9CgZh3H0r83p%2BOMC%2Fd1Cu9xoU2ALwEl6psc4eW%2Ft82AiBRhZmW4QD2OvJin81ht2rpt4DHANbVpC8SucKw9BBtvir%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMx4PLUWAw5lMh4OeLKtwDCK6%2Bx3ZlUykz1BXY6rreNE9%2FOTsebeHJwkoZVzY2OUyHXO84uOVabwI0SNeMN%2Fv3HD1KAvcDMFa2dv4cu4xb21aiFI9PPHtJbhzvZSXiovIx%2Bdx7CoCP3zldSm35zPTMhry2Zuhs6JBVMu8BnsOYTd536yuKSGtbqNd6Yk8XwkL3cuSOuS9LhocRU28sKqS1NBWggRarmCMZslFzOEWhQJUmNpY1l2rfCOL7tEMKdJfOiXOIbxSs7oYCtgeu%2FD31Kz61ZfxQKSvV1TeIUMw7p23gxMiUXtg%2FE7AfqTfVNZtvwrlcQv0VFQ7%2F%2Buxrhw4Q7LC0ZIXxEKY1Hh2JUVTloSy3rXEkP%2FPZNpHes24kAc7Ps68we1zGH99dVbWU%2BiYcRupK2pmuXEmr%2Fhi5BZIMwze%2FwB10xiU2zn4WVSnfMx98leibcNG4AUMPJlcAs8rnqMEQvetqQ8nC9Xk8eNnxQg%2Bb9%2Fji8TRuwgYANRcBxOr2bbzBo21liWrxFMDuYnFRvUJidgKtO2Ygr1l8rDRJR4HWVyWPMqGAdi6yzKVyg9erZyskn9qU2gMimSWwhnLU6CNQUpj%2BidQoZSE60zK5b8RBifCMx%2BzMOFiwAVbBZrugH6Q1G9Peirwjwa0wldDQyAY6pgH44bPOCIVw9lbwYuLGX8hu9WFjTYelcRwL%2BUyLzmCu5%2FegSjW06qAvVqzjBXPtR8w%2BSQxmPZCCThrhZXMBwIPifq8PTCqgq6347wc4Z9tbwdSRRNgvgrZ6Wp1l0R6KaWV%2BAmzKDOCUOYZIEza%2BHcVUeu7cFNQvPMl6V4Km8PLdes1mkbEuaOEUhl3qQyZojRyJGTo4OmxjRl%2FPvJdkzLmU2b%2Bfkqwz&X-Amz-Signature=1a24a7a34225849953e7cf2fd5acc8e18f2d3d00f8ef3f24ba95328cb147aef3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
