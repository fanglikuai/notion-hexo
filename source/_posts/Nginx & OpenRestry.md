---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ECY2HUW%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ2OQaU8izDq9rvbLZP7c4JNOG7eYRgLDO28VgYtz0kAIgTLE17EyIbAJzNEEIf7y%2FznANpGoL8PzKKZGdEy1sSmIqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ6UoxUsqvvoV5b7XyrcA4%2FypfAkjfE2WBnvi%2FQKlzBCHVLmESGi2pbJEvsiBEGZR%2FqWMVx6nOoD7zbL6MDm7VGCztkSQVjvkWPhUnIrgDbwrS8NtJFJz2TyJk%2B%2BWW6S%2Fu1WA%2FOGPgeVbfxUUErS6YeDQUR6Ai5TNtcvPljH6JVnJ8iSqVnX%2FUQ7nRQfW2vv32uHDKIHo%2BewKBPfKwSMaT1ZnV7Wip7XGhTGJV2WsZoPR2lfiQb9BZLQw2S2b69PrS3%2FdWRggeCwjMHKCOeMCOVAa97zIKG0SJ2QFpmrhXFGPyWGqukCWLp9qAUTo1NFElD9OeMITSMZ2cl4VzYlGVhsRNEqcDqM%2B9MzXA0yt6ITeFoj%2Fx5XoNOPqCOb5%2B7qUzunEN97UjHaSq5YOJBSpOOAKNT%2FtdVwvR8Kv67Uq7h3PXVlQbyRi8ie%2F4CK2YOkbA4Dtk6RDj6u0WolZZhLlvurEqlPpxbAwxnhK9wlFTSQkfvo4%2BN94VhoowuLPs%2BMjc0Wq5J6Y%2BeWOpNDWpHGIMNCpuNr%2BlCbge%2FIGyje0WMRs70VcQ7GW9rEEETgM8Tusm7vNx7%2BEIq9%2FUCbIKqONMlQq4Glza2c2hEgvhRNJq8NynHEmy69UnNPGgQkADKdDhRilJc0otnchTXBMLC62MYGOqUBSOKGxsPu9kDDAibp7lw8JEmdo%2FnnusfTl%2BIlxdoEJWK7i%2B0KrpxqPNLhELLFYalA3gBXjqtCe6rmMuS%2BA5LRHxoYS9mhXIGqWoImQR9WIJ6REpFysiSJy82pwSzkMIgezK1wk%2BST7%2FtmUr0B3UEP6rQv%2BuIFBhVf7%2FAvFycHHYrlLH1MrtCuC9vumzjDCOAut%2BuQ7BZ4aSQJ8Ottwu%2BVpGDDsKVp&X-Amz-Signature=f429b876a35695e1ed40874aebf5ad465a7ca2dd7ba0f643c86c65673616419f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
