---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664D5ENW5A%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T090134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIAd3UiecPg17ZGSMjv0rwIW7GhVVghDRWgXKuZrGPwTxAiAweoIPMo4n7aEa%2B2wONWhb71%2FitEh3sKaLOiNmnxPvziqIBAjO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeo5zRH9048khddh0KtwDvvY%2F%2BuA3ibXhNPOCDFCOXEH6ouljPeARKkwENc0mEpczSw%2BxG3GMsAVMaxsgpLox7KATLb58%2BsmMxRPpBfiCFdMFVprQdj5OgLXzSvCUef%2F06QEslZJhAa3tBo%2FCNj82Z4nWOLrB3ub4%2BX04JaiYSWY9Zlqit%2FF8NoqQod3Jfvu4AfeKNb5fOVJ%2FJ%2B1gf869PYpApuJ%2BZV59H37vLq%2B2Do2STs9qTrR9R6TWfrL4YKr1orzs%2FJ7%2F8Bn5RvaZ05QGlmBeIP05wqEWJZN3qMGX29IWOSF3DmrCMwqVf0RXfj8p67yfiIxlWgs3ixtECBDGLX8wEcUo88ulMKbjRSG6AZh57IpqUTqeCtmeIM5w%2FMVd19NKsfWgwuzJMk7%2Faf%2FmT%2Fd7K3dL0LTe%2B30ExXWWIqRuc9VL0uo0WVGBpUCp%2BQCDf5HnvFiu16GIM1Ctt4z3RP5WftVMW5Rt%2FoPCKA37F0fwqZDOmhp1yKl6PhNRmVBJCqVrXvKkV3uQStM0KWhCCRToSjOiAVd8873s8X1kFDFezIHvBE9jgakh7dSuPw3yWpgTJ%2BB9Rx%2Fm%2BC5LQSjv16m6YaxXgznKKR41WFDHGvsGyNJxykWFklMnptbAXJt2fATMVEIktrpKkngw9bazxgY6pgH4lMYk%2Fj%2BsANx4N1W95G%2FYc3lRCOMGN8bAtFBZ6LswPCZIdB4pwa6gKl3XWFLDD3pa%2Ff%2BL8Xxykp3725mB3Tsf7rQVV5DjbbHJQW17Aoyx3V0RdtMnh4MXNCniAM2WYi79omzQ0KSaJho2uFHEa1J1Epc7bdgkgulGxashHki9VwjLLD22lrw7Boe2wxj3uuENS5C9mK5jKbElH1Uo9CIDOOzgWBeI&X-Amz-Signature=9b53e69fe819f9bb4eb7573f79812aeb048e68b109a0ed446cc3077cdd33fd73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
