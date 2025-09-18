---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWCIA2EP%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIDGWL5aTONU91RJHWlwBdUraf1JKY20HhTDIyQpgwwVfAiEAvgjPkm4T2OBiWNWqDYJWusgYNu2B5zZbMMUbWwykC60qiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOugUfp90UJENbjLCrcA7SsIQlpJDsUuL68zqOzBbC1LVZo4j%2BG4s437PAJesqc1sz1rfq5ed9SipyIvmh6U13uBeR4tgwukDmWtMyjSWfafujp%2BKw9MBsXiba6%2BbSXEOIPrr%2BJejAUetvOjgN%2Bu8ij0EctzRd%2FQEgkDu2L%2BnConr%2Bj8haKdcImJBgY4k6u6q5%2BBFNjl9ebRhupkFyB00w2IpC%2BH%2BrjETuWcE2SjRcllWqXmMTMWIJNhBYcFXsf34Yk6dLrRY73nKKd8Tyc1p%2B5VtUvhYCektxO0aVgWH2akTb%2FaftCP1EywabIB6y6y7E%2FHBzqcvU%2BOuaf0S%2BBXzuzmK49IitCJ48DJIM%2BTAR1vNkGc7GfrviDULsTOhFiIN2kaZEAJD%2BODejljZBb9h2MglDY%2FYNvfEdY5gLryjrIgUThqqbUSlL0juHDBUajbjbt5%2FTmOU3oA2CB4PfYz2SwQV1TNTuZbx2vJFByFZJn%2FTig1LI%2BhEw8FoM%2FdljgDDmAmbWuVpUtmuCJodY5fArMvKD32%2FpG2SxtGwkbdEYSQX1ZgW5ZBRi%2FTmOPlzvmduE4ndjn8Gh4JTDo9lRKRG%2FdHOf%2B8Pd%2BS4S8epvE%2BoJF29U%2BKLS21rO6n%2FwQh%2B73utXsvUUldX8QeOflMKbfr8YGOqUB99av0G4rfhPh6bAjCoPCwKSIcZuk7TODu%2F8wvxYdArrdeT6DFLhVcoe9e7WsXsSEkPIQfMzwIb%2FwiWLI71iF95cHonbvI%2FWpe5y9JkJEqYZsWPEP6MZ5SJs3RFQL0PCT%2BpjAR7gd%2FTZVmHnarY5TZJzl3vlOQyRWj8zMjUvXOuawh8uC75jgbwLCYvzxbu93z1eBjYtOSKZDQ4zJ1ZObVP96O%2FLY&X-Amz-Signature=9b3768286c57bb900ab3469b98e7a1f1838c481269fcf60ab1c991de7b0bfd2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
