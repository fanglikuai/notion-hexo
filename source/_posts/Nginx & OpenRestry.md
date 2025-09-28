---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVPLTD6K%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQC2ASy%2F%2BWIxgsBF3xIiXKL2aduNQeWkB3pdXdESKka4ngIhAJM7BdhfJhqX5WsD90Z%2BGS2B5fJ%2Fe6me0WpLFr6aQa4CKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwa9Yck0piaLHHpXPIq3AMWISM4OgOvQ%2B%2BozwmPKjbUAMKB2tUHjD%2BVqHwU4XZzoz3NWNt0oBZjB8iH6yudFzUyLpZS%2BlLNC3mMUlVa9NMz5b7e2yCbDzFaAoFgPPERlHxJfjHUDH6%2BhusNGHJbUaDEPvQ%2FlQN1a1xK%2Fn%2Fid8COgP5CFcBNTSp2zGEXboCSYMi%2FxpE1ySyNu81LDykThm%2FBm2pCcwMTQ4sXn1jvoxJ8Accsqg2s0mcDK5lXYJWeqstUDZr8w%2BgQmDtBysQrjAf%2BcPV2G9MRieuamq30QVz2Vy0Crvs%2BX9AlgXqp5PRH1uL3sF5lkjSZpaftDJqDrYBI9kMwCxUCvdRVTl0D1hPTfClqzxYjH6Q7sV5Xfsvksz81Uq0VEQoG0H1anCHKGx6lriwPUOirXp4gMAgJlVB8DkyJ%2Fe4WGLB0JMNpAOzecKqnAvJrrdE8sQUWJTIP4zQsbQee2YwVOkO8MD%2FuzEVMhKzY9iGLy3Z3fdZkeB824jK7VApBXunzqcK4D35HWd2pfXX7I%2FHEG9OjM3M4FW4twT2LmHqCLjAsIsVxCDNX7cFGRJAbNu7U1wntsI5oMH6u317oIsyTXsymvvdcpiL5DdndwgDXd2H0OSwdUa81sV4%2Bm2%2BPCEUxGILTTzDwqeHGBjqkAYt4IbyHPiYmbRegYjiTgJqPrKWmJGa05K%2Bj5n0%2BYUJxEguFp0qQmC%2FT9B%2B8JQQaoRyQLUgpnm5Wpl%2BQxNI6aMihlCB7NXduA%2Fb8nqdMAd9YCcZj8Te1oafRRwCFUmaHQS5NeLrHSN8TpXBzxIkWGP93ZyXFD%2FwpgG9q8P%2BYDTPhaxNIFIBfhmnQ6JD8QJSXQNgAptuYya3JpjeFhwHg%2BkonaT2v&X-Amz-Signature=05571db92edf5bf43d00b558fd78e81c7ccd5ddaeb7126d3ee3a94edfe358249&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
