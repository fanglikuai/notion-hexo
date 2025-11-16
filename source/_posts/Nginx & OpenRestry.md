---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQ42PURU%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCyff120Hs7fsVJVX8gsUf9ohDCtIpYvolY7LkSNHayMgIhAMBHpMSn2zxp5MsCeXPq7fV4Wqxainp%2Bga0ckfb17v6ZKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwlco0gkYHDR%2BSBcRwq3ANfhg%2B9tF0iUBmKD7B5Ml94lX9vxwjBFWB%2BRDHxKH%2FNU9pWUsJdSUrJyKOnLGEoRUHmeGEEfk%2BttmVG0A7f6QZgB2gwhw4NrHuR0aQEBV%2B4oLPMUT3qU%2BDGlx4X6HRMsvu2RWISzRNjf3Bo08e8HZuKMUgdabM0IaYfG9ObwMX30LgceoMzi8VulTCToBkMwZlPQN807Bzl5AMgiTMbmMg4wbGlq%2Be1PXecrqharQuGn8T0DDQkEcemqUX022qLtilXWyHVnkIPuCJ4aDUUk9Tq%2FVmyUZKb%2FUKojomLjiJI6G8GNpFR5EO%2FtCSRGQCenJfg4TpCCgtKTp%2BnZPH0sqa1zS2HTR%2FYmPbVEqvBR5QqG8Gzue%2FoIFLCWx4Atukm%2FapU4dMhzDmva37jLcYIoYlrqLbAyy6JcnN24LFqZomGoP1IdNSb%2Fg1ELWVq3NFjmf0sGLcpeNcB50L7wiUCNwdRqAK%2F%2BLxQERPvNftF1yrQRjiZYjQv0V6ckNVs1oZMbtIdgO3HDO%2FTysuTsr%2Bu%2FZ2U3ou83bdYag8FbU3FNuqVbe4eqPGsobT3CidoodvRKp204JRk9hR3D4O68kmoD9AWblWfr7TJCS1Y1Mp7bRmcgb9XnYhnl1T45hOeLDDvzuTIBjqkAX32iVyxmhH5nL89p5Z6lUqI%2FbdkJ5o69Ya5VC2kNwlQbb11U8aX7WOWA5U9s5LhSq56wAzClUjRl2r9QlFEv9XG8N03qxwUXPm%2F4%2FEehiA6mdhLhDL2EKsMthfdESnNEGb8S%2BAK%2FTJcuTgA7%2FzmFML04R7SVCHo9qPaAwaWo4WI0BFQUXdUqeZNbvBw2W%2BsGexsjE4INzjcz7De6HmOMUtvOTRd&X-Amz-Signature=8e8c40456c18e19deb06858f6615755f908566accca03d64836aa1f6597ea042&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
