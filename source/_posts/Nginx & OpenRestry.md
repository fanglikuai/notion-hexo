---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A7VJP5I%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T060041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCN3QuZ4SEx5DkjGWMW5Nmc%2FW3yEqQRJQCwS%2FmSCRngGgIhAJ%2B93WnGxD%2BoIDxI0OdAnwiqlhyYM6YS31ilDtBqpIwKKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx6UzHNovvTzsWNB%2FEq3AOt6Ro9tdYvceU02oO1wBNK59kF7JjQN49obBA83ZI4WgrIiKQoVBUMm70Vi86uULpYhe5O%2FG98M8mZFH1xptDL%2F2SmYvxDxBzMixQ2Irl3036CIwzVMK3LicW5H9iTj4GvNi3EM6nTylikPwpEefjm1qMxtbY%2FwPx5W8NusUuObwioXckgWhoNteWhtFA5P4PvSrhYa%2F3yYaIVQDq5J3i%2BFDFs5z3YSG7j%2By5nIg5CsJsAccFytCXNGT2fTkyR8BRau0q7aryOTeocEs7Cebf0PJ2OJ6ttdaeB9I1YyaPun3RmaWEaLGLajDlnyGxyA14c9qKaFaRCxWeCiq55chRxRgysKl7Y2yS%2F5KdMHeZTuv7xqcFWzUg20rT%2F2FJPQ04P7n8xR9ylvL4Y36mri1H7ZINoHGCzpiBps%2BVf8ijWpA0%2FlC%2Bmp9OkN11uZ7P8lnREgYPP22E7r8HgBKAsDUQYW4ZPF82o16yo8qwmBj3Hz5GbPzQ0ZWDToW5EtZdX6plRZFW%2FxcAbGFjoHbUb%2BIDNGcPgcw%2B2xKRgtMKXxz0CIEb2mPS%2F8LlEtuJiNYP1Ecr2L2%2BdRwqrwzCX4RFeMtOXBAyh4cc7bO9yoFDZOL42HqjpHSrRkcfgnKWfvTC5u9jGBjqkATXrId9M4SHAX53%2F0VRTAnWrDfpzlyyw5x2GKW9pfbh4MK%2BEoogkjdajKSqphsiWDBzFUiJS5Kcc7mkF00P%2FvTcx8hTW4D2SkfWJYpDOwP76gNSJbGwDPXjM3DNtpqgT03OQh0QRb%2FyKrn%2BNxIlY2NjgMaw2C4LoEHe8XXP0blKrkzbaP4v%2BFfOnKVPGFmp3E0cDmmb35KcAljK26oPx4q6sW3vy&X-Amz-Signature=db12391e86d0fc43cffda1eba37fc3a31c5f42e994f8f855ba0b59b891f627fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
