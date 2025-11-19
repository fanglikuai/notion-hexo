---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666OPBI6DU%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T060037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJHMEUCIQDdkqyVfBgMo8pVaPErocfCUCnppKOPrOcdjQr6p0sHMgIgCciwBCvSAqgoSJZJsp44lRVbQrt2%2FrmXNcuZadOGNSgqiAQI1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGAR89Cmi5zO9QunmyrcA2n9G8uyVj6h8HYCHL74Mnny7zH8eNZs5B6l60WvW1aoQFpa4VuhiYu5mfAOZwH%2B%2FrPQM9oV7EwgZQcLQGqpRdRE1NEofPE14iJbWyuBLkCC%2BUO2tEZz83Yd%2FYjjprib%2BhxeY%2BRyZjZyv230bnPBq8C8H%2FuWXqwU5k8KPfs1S5MuZ4X0CR3ppXOMwuMxEO7GaoMrYJlaFX7BPf%2BNvW4j%2BsrY5tlKSryMqfXkxZzn4shtpAR4XavMhpgNzIzA9HSASZTzN%2BXP0CUPfclv1upFB2HaBb33OjwWXuOEn7kqWms9kD6HNtR%2FtFuyMi0FhsJuWco0KyQL%2BBk5uJQSV7BVzR92jmtQfpfscoD1nmxZ3DrEqEjb4PaxXB0KdkLQac3WMnkUa4DJ8R%2BEYQF0OM9WSnBUsfA7NX2ewSewSmognrFuIbJefFwVEd2Ccrcj832nrngqwuF2rhk9PBWmyl2P0fdFaFPavSAum9J32B3aPwXJmP48oSSwWI4TcMmfNwqJxCVgUo1EX8GfocTEtOqUSCb4W9ylHS2oLInrxEjI1LMOI3YAxMLCQ0vimQVcFX6C0%2BQRTqPECGe9Euf1PkvUX%2BcyBXtj3WnQHQUjILLO3hsmzaMFmPLhSnFNarxcMJGz9cgGOqUBRsBNwp8IqHz0hipGqavWcWKleqol6rFXzUQf95%2B%2FjOgOFvGgXTE7oyozPrQK3jkB%2FQ6KSLkAYYigFwwW2Y%2B%2BvE22EvuwbEo6dXwSXHgcQI7AUawIvjTcq2fYHPNsSdY9jSJ6sFmeyZo4yj8clw4zz7X1TPTKDO203KAUKnWR5ZeQjTuf3HmYZJBAQ3r0SAEvibVZ8U8x8ZQyl60Sh05NGYcUUhqV&X-Amz-Signature=3fec281d3c91cf3da88570b1ca2876560f6eb3fc6aa7e2b84b017081760534f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
