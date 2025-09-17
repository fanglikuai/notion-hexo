---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQO4QHSK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIFnQb8oLnxhlxvkPXog7umtzy72MSTB0GWuBFcwXF36SAiEA5weTpRpvwDVl9PlU091%2FCLGjYvTD85ZrzS0jdhIk6XkqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIuQR78HjJd5FUcZbircA2EGCoPC9Z1%2BZ9JOojymoVzA%2Bmg6sjTJPR71w1miBUjACiTv3KYMaaN%2FpQM5HXeFLC8WSqui7Yt2P1VHHfBSBjONGboAmT5aWgOcpAw8oqg21GSutEKihcCzLdy0G3QQZD8fpsPQecPT2hZrizpkN0MsiGLrq22MuhJt0ifYDQGZCMikaBoz%2FR4tHye6JpzzMhvRKZCd34ASmWEbeKWImkPpQWsANkDPMeEXy%2Bll2%2Fqod8NhQKcXdJ3USSr2ZdcAo%2FpjvYnNDUJJZkEMtgYq2%2BSIV2XAOTnQUDgYreeY5riJqbvvq%2B%2Fa%2BKrKzYadZrkcR%2FNytRjt5wSTEfenbMRi7jEYToWS4ngRDif8gfRdsKt9hgBP0cwaJQuyXbbRB2gcS5vj%2BAsiiiCp8Ko6FKwwqldhNxR7DzuIlMEv24w2bPnCGgVjkT%2B%2BWyJ7%2FLaNJkT7Fn7n0ce8cK%2BA5%2Bc9NFgD%2FRFBTm5gfCJv2bp6cWPregP0OvbVyaohbAr80ZsgELwEHEKBP%2BzwVSjIX17vtEUWH%2BTcxJKi6V3Jy%2FFL8YSKTAnM%2FFXH3xx8PbUT1d2Rjx1ABSsmtyQdaOIsAycgTJw10HKI%2B8sMbAiJtPdf4JDkTgqEwivVd67vi4tuSSeUMP3Xq8YGOqUB26x1%2FUU4SPP%2BjHqmmbYyUjlFa%2BKb5tkT1VOnoJDBoL4zRDS1gNwGYgzCJKbF1IRvwH6mxo02usNplr9xEs1zdhK7SRxSVeg4imaGLNMOmDvrkThpSCNV6MTBryb36MmH851bA1fmjU%2BWw3rDEIqEPw0bL8iQ1oFiuoCeVhfNoOJoIwZPxn0dYycj43fSP8mMS6m99bfvbqWv8sllM%2FB9zLIdHHjG&X-Amz-Signature=a27930d8bd7e8b357759989b8c350a6d818d491d43e79e0c519e8b8704ead000&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
