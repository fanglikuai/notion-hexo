---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDBEFVER%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDw49VqDZid%2FY4w3UOalarWnTxo26SKVgc3BEHsPHxSpAiEAz8HjfoH5SvfcEWPIexvC5inV3h0iy9koYU5bzfbs7AcqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDfvXPFXAL0hOt2pZyrcA7HLCi0B6IJPRtXLBQSGHRoiVY7nCN57fWoeBVyEXW8bxN%2BgurPMC51Rst%2FH8DZ%2BIC6GJgMUvuY3TnO8yokQPhQdb1qLVU1xSU%2BnBEyMX4tEqdUw5xrdr9VaRkTyrjWDfLX9u%2B2ooH0%2Fdw8IPLBpB1%2BtylH38GAUXTKm8qVuXd4fOqvdPIqcQqYMa3z2d%2BaeumYERZtUzphElrBzQH3ohmz1zPl4jYTgz72EL3FrPPZC1vzi%2BZAtw%2F955bFvvx%2Fp4pXp6BGAdnPRYPa3zq6PgoJ2MHCQ39SM6Z34zo%2BECjoSF26l11Bx3kcbYuSs1R%2BgpUeHiGzvWe6mJipWusCy2TmdjkAMm0WS3i9nrhrbe7I8rvOKUUx0qIeXL02hyEG2EJwIAIPNyeAFFKATDq3ByCs2fisjz0sHod9UKTd4C1roGmio0z%2FVNqPLDCG4cfMdDBe72Cd5QU%2Fw1Kc0a3YdenJJyR%2B%2BvuKsz8owV5LatVvooQGFJyxkAlvtRmfDRArlFbKKMTtLTMLQjaIKVytHYtR5%2F%2FdCZYuGYa35sz9O2Lzp7eCfZfWmK1ffljRHXWODIeosE2dWIV%2BDfGRaJUag5UfxAzDydSmDn4rURs%2FvZlzflFndCbGp%2Bq%2BmgfhmMPr%2Bi8cGOqUBR%2FL6u7NW9cK8CDHjKec6umIGm8S3Wq8Y4sne0A780uswrYrKSfuvUHFoCGVO9DhEHbryuuvZVdaKknysQIE78qxTPA52BRoGSQdl%2FTwzMHj4BkXJb2Hk8vRWUFGfqlLEcA5cvDhzn0QjZVCaLYmv3z1HRh1dO%2F0E2Mx%2B41xzVc5bFvNhpfnsnWixXUPjt408SjMtoo%2FtzKUWqxGtPFtdfjb6jEPe&X-Amz-Signature=3f3103da1bd013ef23a0fae3beaf6baf9161e403cc42e04f1937ba44ae585b6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
