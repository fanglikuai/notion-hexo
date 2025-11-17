---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667M2SVHDE%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDt8%2B%2FclGcVdX1Njs1ND83bdQRNIERLyztTQUk6rdfsRgIgfx61T33tB8dMyooD3EBz1%2BdExD%2ByecQS1npj8upjb7sqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEQuzUrkd%2FhTpyvMvyrcAyl4FS7lDvd7JoBCUKJgFiAvdbId%2BqQmTQnDoRgvoivjCMi9lD8YSzE5WPM5doLah2um2QQqtAZ%2FgdIPrLzgW3WPi16W7SDTnR6d2Sjoydq5H0qJUQd0lfHkXdvQ%2F0U%2F6xm2Xh3Kxs7%2FPbZLSMgL3CTF1rQq5s39SIbSjex86erlJAW7Yw3K1OaBBOHf8zY3u8J3gc14u2%2BcbP%2FI%2BNY2JGg%2FLWdjQLPXAbHT6w1rcDz8NNEjB%2FB155MtmSdjCv%2FBXqS0XA0sdKtMIgTWIWF%2F4dAz%2FxnooU3b05H6kGUqLo55BzqxXZZ69QlOPq9vxpeLOwkefV5z6SccUCDbhELkNcUFFNIrKenm5AvMCLyFdkIhYKhwtb%2FX9LtVbm0qpaXb%2BtwS5o5h26tTTQcusxAUS3A0lEtnWd%2F%2B3zg7YAretlQ77q5u5qiGmdmFrqLYR02u%2FF743Hs5eDwEd8kH9CPElZVsPoOKlPQDxkPLbA74jKpbBUiKQITZ%2B7lwJIPqN9vGuMgxdQEhmNWBAQPl2soO2y67Z%2FTd6Ed7vER0y%2Fabgp5dybpJDZG%2BAweKfEijcRtCBR1RlJy%2F6J5V2vCTyv8NNsTJBsJCfVsMbHmysdkqk5QJdPmFvyn%2Fs0%2FwXio%2BMIv168gGOqUBy%2BrNiB6TA88WIZnsgKFb7swXX81tf7G%2FSlQfxdbZXhpbAanSXJuzDO85X%2Fy0RuL9fyzo3OuY70lCXRGwOZaRgdZBPCIOoq6PbchcsX9aKO2RHGDxCT4PAzMNLRcH8G9A0f%2Fa0vckVBne%2BYXS2wjHPeW58Xx9UwLisRSfDX5B18xniJwD3Lk7r2nCkLJVFKmvbMudIC%2BWuXAk5sscCVUVwauSdckS&X-Amz-Signature=bfd4e4104ec247e1ea1667f5a8675fa3a53cab25952fca34d2f511ed461b338e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
