---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YV7SBHP7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICmujF%2BGHHm3lOP5SW%2F2wp3Jf9L0F5c80rfAoyX%2BTd%2FuAiEA3rlTBW2U2y5Hm6D3lHEGpADi30iOXp%2BoMB%2B5MuCy5rMqiAQIlP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIydr%2BQtcqfF8LiOJCrcA7WEgkadO8sOI%2FC2OjTqo%2F2p%2BcQeKtgv2z2sNWqew0S1QD80cb2w2Qk0OCLw0w2wQpMZjwta2Ev%2Fhakj9pqOeuhMb%2FkXMBHrJuVMCndXaEaFnrwaouGd3r10YQIrhjIpKRKhdS948tg6i0xqtqM7dLYGYQXPn5obguGcjp053fP%2FEE%2FKrjS5qDwDCV6m3Rn8%2FNm8eU5D92sNHM2r1jAU85xGBUZcx%2B5tJfSaYk0jaSkHP%2Fhfg3elV%2FQV2rWrARw3PH1sJzNIViSjFcgYmQfWm1liRCI9LZ%2B%2BDnSi2GK3zRpwnvYPyk3cMkmLd5eqBT4WnHRzazhYV9rUuwgFzzsSpwUOjssZFlHlQ91zUi%2BOGHeQHwotOg5VOpCewXcMBdUpHI3aMpF%2FspGc76HGDJ7AXjRmQkQXCyNSKukf4d8r7yrJQEDutRTroJPo%2F10QW%2BjdlNgcRlFtRBr7o7JzSFqop0x029W0w1Q21aNu1ql%2FhT2QQqSVZulv872iAAW6yg4p6v%2BSAMnKnjwQoaPedUH4nM2nhLJ4qEtekFlhiWiBY1ainEhy9Vb4OMXDi4LmSH1CBiLVRE1EeErtgHm6UuTkzAdEspgrhJWASBCPVGz2XFECd%2BPj4i2Z1Ra2LiJ%2BMIiekMcGOqUBZbCoqvbSga8YuHO%2B9VhSVGwA%2BC0WZyiZwwnq3LPq%2Fh9DSNvNb4kp6JFVupCp1Pu40cxJPzJzTLqwPb2bfdshJVKkzobFZquh0cwCZ3AQ%2BJW1azucH44i2GH2afBkiTMZGSJC2W9PGooWNok1e0YlHROaSMphE%2BF6T0pdtSijjnk4fircsySvRnUXaJ4d5W3Z6iT%2B7E5Yp6Jebz0gOtOfLty%2BUix2&X-Amz-Signature=5d4b2999984b7887883f4d67f9fb601d0c3e6ffb17477f9107d4ce6859a7439c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
