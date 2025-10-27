---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TCXT32D%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcyWEh9MWVLOnRPDrM9UU%2BhY%2BAY%2BiM08t9e05KTEL1GQIgGM%2F%2BHut8OjDZf4R1dtBsxzS0a3CO32R26%2BIw6znfY6QqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOeapAQs0y0vZtOcoSrcA3id8eIrH4Q3y9tI9t8LffEdyKin7lJfUMTRmaLKxQridvWdh9WDQo2VqYyIDICsdQU1hM%2F8MIWJ%2BB%2FiTf81gPpncTo%2BJQ431ieQAAQ7VfbY4IjkJ6%2BQG8CLuKejGufP6rEe34Bwd76qMVnaNXoW4IbW9heKrk6tiWI0Vu2QE%2FQGxJxFG8VyWSjwz17Xu9kbD%2FiI%2FENmfaWsy5g9H9lX%2Fq%2FdXsv0di2CE4yzhwoPoxQNZ0S38ZvGr1vD%2Fet0uS8nymwMzZdMj28V34ZhjALtud%2B1DHneNK%2F%2BspCF0yHI5uMZulwRrtJpiTVzLgsSqUEVQDEpBGVIsBZfFZ1BcvuJAtoW2AnogKIDrR8hsHZjkSri9YOMlAhSDB4%2FYDJ%2Bqkl0o7Gksg6cEObTxbzOxJgm4H4Dn3xoHcd8edgr0sbnFyxkUfUcZXSdG3MT7F50E7XFPoJtyW193iiUfjyzMlm8Z80e%2FL8SgIE2PqTKcqXtiC%2FQOiqw%2FBuTY5vXI19pfEFqfBUxeCtI8KTaC7c0Jnslktmx3Sb2058h3BpEM1FNAaHq1mQfwJSgtc%2BvuP7dS6%2BB3crO%2FeRupL0b256txQWHvi7E78N8jhz3sZmasNsA2LYd5JhCLGULLM%2BnIRQNMPuR%2FMcGOqUBbiQAhcy2UEPF6woHoLe6Ro5xF3bh%2FXs%2BS9YSZpXche9T2ET2rjzav5rG6BlN0nLcMfIgOfiQoMYzUXu85luofYKGuRTWXpX%2FuG1JbCSAAN2fSDNTFl6vpIg%2BGLkb%2Ff5Mq40lz73cQUS8c6ahmH4s5qm3PXnmw7bjDX8oywv9giiyzu9OSDI5VUb9jClz2Ubm%2BFc6qemmOY3yWxx%2F9KDA1cg%2BTjuq&X-Amz-Signature=b3affc8637d7ce6450ae730bb6099cd8f1c2a4e91d6f95475443c6454c556709&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
