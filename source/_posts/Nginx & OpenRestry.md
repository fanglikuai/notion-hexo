---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632HGTDNZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEa8h42jBlglnvtPz6VfkFSI%2FQ2rOFwLPn0Pd3YJMeF2AiAkBM3kCGsSthckv7kaHL2A9gq5kZ%2BQFfQL7W2tJ7cvLyr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIM3TOJrFMTV48ODYGCKtwDqelqNSP07%2BmAT0GCFrpPbJwTIfhoaqu8NA%2BVsVDyVgbIcCbMPFpbv3LRXSX2NII6cZmYeNSIFW8x6DvK6XWKMnS1zZYUGjDy4wUF1%2Bh%2BC30q2wPgLBk7PxOCm9t%2Fo2I6i6cX%2BEWIpa7JDCG96nAIb%2Br3vYHH9YSB9xPeh%2FglflDNFl9redkm5Znj86DLRBivrVwfcQ5GuMWdfuFRWZW3kS41JPP11yD4dtxCYXVglBbSbUXz2E0QO584nQnqNVS5N%2BX3O6C8tVMaYxCGGdb4YH02a9u7Urrze%2Faaf6wP01vN88jpaBwrmj%2FzUIDK%2BgqBxmfvrtH2R7cMt9%2BEnOj1tqF5YRe2673Cnnhu0PM53i73tAYnOUn2CGBryhO3dR2Cq%2BJW66Os5MC8Q6H%2BFARxCxl4j3VBlyX8WBdtd5Hih6JKle1oFga4DX5TyvQvJWQhKSRaooADF7tNQN6ycLxCbxsYT%2F%2FJdBXSVw7SvTmj94qnKwakjYGto1EFJvwoLooqLdWajdXmJ5V4GdXM3kEWs1JzSbtf12pEwTLSWzDsf76ViNpZDqTqngKqTM21UTWZCB9FUuoGSYyNrFRbd21WwS7g4eGNaba4rVI3S7E1EM1JLUXdQu%2BHh%2FOD2Pgw%2FsK6xwY6pgEDgFe177UUxuM6Ap5GhHnxPmyM5YyS0V41HHwlsIn4Z%2BhGMbpowhz1iHwy3xyy86AFXtFmuZ8OEW0FoQsrI7gT1uOfnL7jHfn21k6sK3Jb53kDVqOu6v%2BqzXHkjMgCokwTLR1oMjYG0RSFyVxn%2FdWEguyY%2BbgUjMGWTQz7rwE%2BXYTAnD0XaXwIUv0dKRQzLMKUukG8uc78kKdFRyDNM6Djgm7z3%2BJm&X-Amz-Signature=e0ef0abf4f5bf2fe7c1619ca7c6225edd25674c8efe34b82ba38e310ba89a3b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
