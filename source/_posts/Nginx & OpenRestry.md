---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZR3HQVTW%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECUaCXVzLXdlc3QtMiJGMEQCIG7Y4K%2FrCbd7%2FWtoPmSKTTZ3PL6xobOiM3GcdZQbsom5AiAXO3ft6%2FpSxLTqdVB%2Fqum%2BBMTBhdbrBhRZqpyzmZTV%2FSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM4%2FvTnIZ0%2FGfblPGzKtwDlKW0298hiyArUsRI9ABelEFevOJBZkH%2FM4076yzKF9g1Wp62cZgiB2qMVlCR%2Fib55ElYuJKcxtN5ln7CmzXLfuWavEB1bpzfJ7MDSLHvS1iFm6ioxUK7N%2FyQ%2BkIfj0J6o5tTn6u0vFlbtoCZLp%2BZ%2FernzdRItTulB%2BdR4OiUDT6P2WmnPDQjOPI2h8WLopRU%2FDVKqiwlEfueeu8%2B%2BQM%2FrqkOcbP5M42vEsP%2FmwZjp0NtVU90fkI282kTMx1ZH2MItnNlZCIvMfFCOuYiwS94H9%2B2I2pM46MPcLlXVPFE6D98gUhQZ%2FoFLkH6bXbwq7wvoqxD4EI76MUVlKMUjk4hsjZlen43tuaQ215rqAq%2Biv5G3o7FvtfXfvTUn%2FXTItPNRTlHBgMIdkiR%2BG4WysqYdJkiuI5VqPnTIMfK8p8xRLdbRgR1m2jrXnFpfpDKmN4hKoFbsCnOyV3K3giO8OGyze74qrA6Uz9dIDQdWKtNQQEW69p2Z1OPe2ztc66jaX%2BwziPCEL2qPx3aexeBfmuqF25NwAYAtgUMy4EGgYXLiDPM1doRDAaO4VFIxoY7bDL1G47uPAVQHOrTLpBHyHLFBH%2F5%2FDjntc3SeWoEzPdQheOYuzUtZZLTV5i5s%2F8wqLv6yAY6pgHU%2BJg54hT6mU7QRzlt5vyZlDHrvx1vdFKi4sglIoVR2Uca1FnbVtjDfp2cbLC34a8mDVMfZukJRfWdb6fD2IpC6tOghLXWO8mQn32Pjz4sA1f0RB2vO6lmNBnH8%2BUBDgf%2BnsxEx1OajCnb6rE8i77JNeRf2dzQoKOsoYjVzxeWOV5kkkYnHxM8edJbHl3PVwSMR810zJTFk0EdXHP%2BPu7H32HooK32&X-Amz-Signature=453ad1cf587a53014ba76a0be7acc74f20e50bcdf9da925cacaf154c750bc6b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
