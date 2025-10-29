---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIAZHTRT%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIFkLKS41DDR1ism0bM6WSWeb19d4yB5MOxnePYNBlIpaAiEApHLnsgFNArbTY0Ha02Tg%2Bzllxs3KsuS5iuMxmom0nUkqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFEsv4ALLCZ26SDnWyrcA4eajXcLSq3PkpVYiB2L1i4t%2FeqronX5t5Cc5bWaWK59QbuDCLutxyGqMN%2B%2FdNsFrMmb5gbsEoIlecOj0jEVBeW%2FWHQfcQbwD79NNLW8s1F3N7vIimHJJIfPBPF0jXXJYvD4W4CHE%2BRs4KjcbaioiLcAqC7LP%2BAJIIjpKvnkG4%2Bj79qEt1avxv2EvvyZ6%2F1pOwp3Ix%2BZbBIHQf6zSYHho%2BnscJp6rsTVzYiqbIUb4ZrDz7jLbd%2FmIxB%2BJwN3h1MAuHNDaB5LZD8V%2Fbogt7fJAENlLxLB6LQ2GO45ThtytdyFog1%2Fa%2FJKr%2FFZGQTmvRDYCOMUr%2Fqu5P942WZ61Q266F%2FRSeJcgK25YHuh%2FeO3wIqGhlulDcl6jeZEg1Ic69tOmMb943R5o3FnK1YP1521qPyA9f4iEYme51%2B74wyZolA87%2BiwmDkxUKyAeclrE7AehBX%2FpXCTf3oDe13oUusFZ%2BJ67uR696KfB9ES47H7U1vABCYKBjns5qv07E62BmzAVO9tBaycVfwd1gZlFVWX2mqOH3%2FWfjRj5fR7N1HUhACiyvYDUP8qlWdzUfUssc7JU2PXYBbz6%2FBkO1ChcFv%2BtNxRhe%2Fs0vHx%2BoBYsFQodb5rt94j%2FWFeQBNtDA5lMNWEhsgGOqUBymVdrFl4NNdHbaWhti7kF3279Bfq4yKr1Dk%2BFOTIfPZWj2siRLRfhpUnQ%2B33PbB3pwxdRsIZXXa%2BOdotgxHlbosMrt0Bdoj%2FO7KmTn1P50l8zcY3X1VI%2BJ4anQcIie%2FvoYxFksJEB7gdRvWg8H%2BpHUnIA1ip%2BR8%2B4DhTen4kbaScqgjxu6JYAGpus3BCg8cYDBAnJ6gmG1tbKVqAiInEHP%2Fmzfph&X-Amz-Signature=4bc06854e085eaf0fe4a762ce469b286f2043bdae9888a322b60fb6b772cdbc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
