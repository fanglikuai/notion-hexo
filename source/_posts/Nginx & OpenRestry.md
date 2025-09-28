---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZW74EFQ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCNRnl100aDxHM1t9cnWMdFXaHqjYVlRQzvoOEhzdJRUQIhALcn%2F1zbz7PDXLEfrAdLXnu9X5%2F0l2Cory9QLzvh69%2F2KogECL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxqekgZj4EWuA0B2mgq3AMf0nMdD0qXm6fCDdMzb4ResG9xt7tS4G%2B9Rj7UHV4ykobRdQdFwrdFskk8JFZBTPrdxxQn5OCf29pV44mN%2FvWXMj0ST9EUwqqAtcxM%2Bj4bJ4%2FEAKgihpnP%2Fa4QU4x%2FROLnBDpW5z9TueAElwfirdAuZtxbJY%2F1IOpOzX18NVSbNLfyaqdCI7eMbVzNuLlqQ%2B4A7%2BFiwU25h80yf%2FA%2F%2BmRm93JRpO191Br31gtj376uFiAUdzzj5qwZQbL%2FMPVOC8CnAw1kkgwrkd7aqnfGimzYsnGdXnAYbQHGSOFhsDExLCFCNmtyE97QX6QIEdNR8kspwJ5%2By%2FNEuDqoLJekBh9c%2BNHpF1htHyqburbObxlakOztRn4v%2FNY2cJAchXObC0emUqa9ABmi5k4%2FVYarXA7en2cJucLu2I8YQqQlYkEmvtVX37zfDeZpfQ6dUt5g6pQCOq6H9k%2BTbv5k9We9wgPqb1ohnHISIpfysyleUnolK3CozU0Q%2FL2QIOB4%2BMyYb50aFpnj1lpi4dUfXn28Olbi%2FY5Tfc7%2FN%2Fb5SAzMac4kY2XpYNAB%2BAFShicQK9CD5wXelbm6MWcR3a%2BkiPHC6vHxDAjTxYo1a2hexI7A4wVKa0f1aiqyN%2FVOcFdCmjCZ7%2BTGBjqkAY9DqvluwQ2brKh47%2FwWqTJf6F9FDMw9Z58hzwHp%2Bs6VWjuXqS1ZBZmDtoYhQ7GBMU4%2BDC95SK7%2FvnW0Bi8a%2BpJRi61AfqqUIjHlXrrxIfPk2htLRyo2us8xlbHo7EE4fy6C39q87FXMfEUtMtm2%2FpaOLdtEI22O%2F9Kbfph%2FfhFbNa47eH0VmNav61TpjpAplsGLpIzhAzeecBdFWuSLsIJYlkD%2F&X-Amz-Signature=9381df351f8b25d11b14a84aa764b959e98de053a41bc282b65d2351d703a95e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
