---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KUFMSMM%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T050055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4HaM2J3hV%2BO2nLz4UAa7ULqi1kOJRBwKPGcPwO8HnIAIhAN3VxvG6Lzh%2BC51Mxvrm6I%2FMSncKPGonKaw3PvnIB%2BsrKv8DCG4QABoMNjM3NDIzMTgzODA1Igwn2EHTaXk3RFGi4eEq3AMaZXwZGECLuM5UHA8%2FLvqPG6mHPPh5FZqLBAbqREgw%2Ff2WaThSaMqnn9TO2ojEnsN7qd5RTBfePJFq%2FcPtOiAlfwA5wweY3I%2FGtN7Iu2V%2FcbiKYWliab4oA0ZxipRoSI6fS%2Bfj%2BuDicxiNvoo6nAniLIhggxxPrTpm85NXPSUxQnchkh8Dqh7f%2B2UZrr5HVyPkbBPu6rbA0n3%2Ba04bRZC9Oc6bbDMqMHJ57evzHyLlIpaJmrIifQtfcd8G%2FhciURua0wGD0ZWCQ%2BOq68oCwxk16ym0U0bYGFzcsvrbZqxer1PK%2FJzOJmvqRiK5XdtNgHhg8XTeVp294I6JhRoaGTHS9UkV4hW4jkX4q4XNhzJwqfI4uJsvEaAK3dSKpTOIoLbcqAwoOGLNWRXdxASEni3LZDTBxkc1qxGLDfidKlIzT158ODOh4YR9Y4DfSXnsQqdOcWqbjN1h1iN0jm0eA4EAAxGIfUVEo0r736d%2BzB6xv7Yh0E0pX8jf%2FtthcDlnwbjr0yeWlTT%2B8UZJCiNT7FOAyB5Pax2cJBhmYWhcilkA2M7hXTj%2BVrcYDgohhdYzIsAqGnZMdHpY14N8RcXTENjaNJKLKvLh%2F52UTXl4mgaOOyeJqDx01CBbz7kzGDCeiKbIBjqkAcBpiJizr0WJ%2FCY5Wsx3HcRpMcQ28NqxTPUTaO1O38N5I96S8pSohf23ksWRaVg6sizMAXjaFKRsJlYSduxZcQ72VyGgU9Ij8E7nzOhGcTnUHox1947r0HLj3Sya9oev0rhc5K5EGGIgOJvxU8vP837AEM0aGPCaiodGRq0rN5blDdSNk3mZZGqfG%2FmDu9YKts2pIIJ%2Byn6cNsyQ2rNphVgPm79k&X-Amz-Signature=bb6e27f6d955ba247e45532f4bb93f156f859b5120659183fb5d1ea9e90cae50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
