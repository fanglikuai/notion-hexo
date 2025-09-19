---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4TZE6XR%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T050113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCICTRB7Dr8Ns8txzPUMvJttQ9ZTEkvX5cJrr2kROtlGl8AiEAzdsRU1LQbDIAWcS63qE%2BfCZCSZE1Tm2oOswYrozsthYqiAQIzf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOtlpy4184MhVgZ0yrcA4yuhv8en6MEnrY5kTJzHsp8lbdwZAwr%2FDiBjN%2Bd86fui1yP5AHM%2Bw5XjgnNb289k32lgQZ1DcQZ8rCMcqcHb2z%2FCaF56VTagTxLdrg85sg%2FkRS9oTeEXLheZAr3S8UzyuK9uYwAoR7dp76h20mgvgoRSbUJ6Iv5BwlLvIyEhdK42BaIqbhCJrM5ROj7HXOPZ9pVbP8rD8pKigSbzJIi6RYTZDcXC578OS0%2BBZx07kwZBk0ayOXUADrrwPjBpizg1QEAfX%2FFcn5nQTCuKEzIBSFktKnXd1KUQa8yrK0eB2gkJbkXRj9S84GwpWVzZ3f1TS6935T8X9Z%2BhUZkzYBcpG3diqeoxD%2BBmCH8Y2ogQXnfsATL50irTtluMmu5JlCW26%2BLbbsPnRnUTY4sOUi3B5UopLDTiayFsrr%2BPZkqO8YkjN3wpQ3%2BwZvY%2FGcQVZZot41kXLaXAJB4nATZdxiXIAaYssc81i0JMTMr3jRbl%2BjqApBDWmVSwbCZijamOXnOrg%2BkNCYaavhKw5muBdvwNZp%2FY7EXxPEffjotVXbaKVxNkls70Reu3Ba1%2BxnKFKenI221RVKXRiMTx8TLwdtFZKGWGVuO44ZVCrv7KPpn5JGoPKxZfdZqts%2B2Kk6TMPC1s8YGOqUBX1IdPBO0hYUKBVBI9KfPJ%2B8KV4oI%2F2lKE2LITe3ihGAzVb1777PZi0z51dOZokaYPl2B4ogPw12ilGp5pmar9vYc%2B7WupSQTYzxbhskaWIoOfTR3hSBjDqMcDbzdeccyLTBkqdBRxJ618ZRCUEEEdOsfzqXy58j%2BLHfHZ05EbCf35GS8Q5RG4kJUAFvLSYBv23JC5OKOkcLShprUezsNKBGbt536&X-Amz-Signature=69fed616a33de0c0b69c7e1d8328887087830cd26179733af6e4618cf5f1374b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
