---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPKUBOC7%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIDOhbZ67Lb6pWFNE0QosUJB%2FZM2WMRKNGxRmuhPlFZm4AiEAgQ2T0u16bhIaLYxxVlC09h79CKGeAvj%2B%2FZrCzU0ByiUq%2FwMIGBAAGgw2Mzc0MjMxODM4MDUiDL5veLwVXU%2BKoWes1CrcA4H1IksJJyzzGkN%2F86KvJFx44AfCSwCINocRHeosahLbfc3MlPiCLoBNxrT2mG0w7mVnPbfwDlUje%2F%2FSfbXXsrZoX9c10so6w5VJqTQ8NiYmWozIZGbro715E4pdcmpbR2gk4MDhxZ8Oyn%2FFBppRoBFklSbqn%2BQvsW3EwK2tezEeJehDYTy574zRpYB1IgvKiBCx4eL93jSmDdN4y6Wjv0QMMVUGYU3N7OEJ553vs5BR2R7NQhzeZBdNWcTaX01MN42cGRCEaBoYvfX4SvDZMskvsX3IJla7H%2BT6U0dhyMZZ2KMZ3qvo7VbGv2T6MGW6QPAI2ekvIeHGT3BSeCWL5abUh04D57%2BfAGFrwdkrq43v5ObyjFO8OP58zB0lbN2kO%2B0pTt7h7kL6AomvoA%2BbCMP5TemsPgRFst3gjm%2Fdz7NERohaH6f1p1oKK0yEB4hFuhs1J%2FXiR1p1g0sZoe3He3GgiIDvB7Ynf%2BCCfMzBGcRELrMsVHQWrFDWlHidNCO7c8jYQjMh8ZM0pTzmkPhq9UCJDLKMR7DqtH1w1hosjXtKshsWDOqSeXmK8Op2Cb%2BSJT4E%2F8i4JkB3hs%2B9HeQ1tuRQ4aqWXyyNb2oovajR0%2FuAwcy3BxH%2FvqWkOhyGMLXBy8gGOqUBUnHhZpl9GTa3YXF3CJyrnZhJVdw6LxjpGHzAdNdo2LMNex067mfnZaoQMiQ0Wi4Be7b4jgV1%2Fw06O0tAPCHArcu%2BUevKBCik9xmKfOx9hoiwgcNJUK1T2NQhfN2h6keNFz6fq0iJoyjAIgclkHqURk2sP83r%2BDAehJbyI6QijYTcfhSvJf0gFm672hhUeIEPanpNZ2MBnYJAe9WGaor4B8pVCtXB&X-Amz-Signature=c4f19c764d0d4bcd42ff3bd5c2ce2da863310c55a202bda588466b75f106d840&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
