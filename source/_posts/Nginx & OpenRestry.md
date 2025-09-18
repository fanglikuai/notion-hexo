---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZF2HVST6%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQCzX2OsExSMtIQE0tSxHsTb7vFOfrERNpH73al6I2WTlwIhALL7iPLSirKOuR7w63NIX7ooX%2FvvtqFTcC7XLxcV2QE%2FKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzrUrdRHoZsPDpRqfwq3AM90W0lO6jhy7NAwLb3Qfo6Ho1U3J89JNpnKzxl6hrmMqYmm3AvVrGKKDerhy9GbA2JFZusE11fDyd4pAuxKTol80S3wgpYfFRHDUPHS0aT4jtRw55PnKsIuiHvZvRk5VE7igsyjE1prNp790nSFh2C8vll%2B8N%2F3%2BFZ%2BheCXQyAJh6wWfIntMMwMNX3cNSwubxOsxS%2BCOSypOoFbM%2BdKXGhYZdLjUglXakffLefYuCXBlV0ggAtSoWVIIcYqA%2FP4Cz%2Fd3oO0BsWJ9EjY9YFKk1TPGvx6voMgLInOVSB%2FnmTtf%2ButiBRMCyUG3%2BtMUWhtPpoA1xqiq2thmwkq0AwBJK9U2osdSUryIyXA7K%2F5RVYYNXnIQvVSpmQ8281WbR%2Fs6hBOyxhTWbgQhYlP0c491Rf7eGYnlfzKFl92pJ6lDcST3Zwc9ql6d7OYPB75f%2BKazxOzuIe6paWbMXVIE3loD2h5x4WIePD6V96Swch7ir71mTmLHXEL1DBlzb6SbrX9QoTlOkefq2UqdMbfA%2Bi1sdWLcoVuH9ElkZHMoLXdAOlm6t7ChO3LM4k7Od4izWtQZnkLVVLFxqeBOhdQ36mJDPnSsggeyQ5yHy80%2BOOggdIqbSV%2FSh4Pn2PyhvgdTC9%2BbDGBjqkAT74r%2Fzp8TS2mqLzowBALjmIcNhuLgWlVV2lPlQW06hs2Lff%2BNgNstDnNkDPOcH8L%2BEWTTtVEdNvDFXDWQEaiUOpucde7DS2r9HQvY2zW9BWzE4Jd%2BSWctOUGjKEsZ0afgHNV9u2Fws5ILPp%2FJSQnIYmbgjiWnPiVSE2nVNSmhilmUHPJJndDK96OWlIRB%2BV9PKeZJcvoAuGwHODknAS6N70nAIQ&X-Amz-Signature=0219c4818d77324f443398222ac9decb7b6e0d699cc181e09c7ed73c0ad3488c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
