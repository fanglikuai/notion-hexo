---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TLCDBYK6%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T080056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJGMEQCICIID%2BVv74%2FArAptGoDjzupJuA%2B5k5n2PDDn5F1X4Va0AiBMDtZDDyhYXPNd66%2FyowDsWrGcpkA8p%2FMPHmMH7GhVJCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHtaGREQuekLPM6Q%2BKtwDrQD%2FU5q4sQghkeqEv%2FuN7HjBZzzEw5E%2FtYCJTkcWShOnzVdeZL4D6kiugBmWhfmOuwVYwDNktIiQ5Wo%2F6tjUBX3BzrGrVVJHcR%2F0255DTv3i1jt00IYuNtFrZwDHIltygDVMynJ0g5ZkEgv1m5qzHv59HEMGvIvQ%2Fz8Wt4tD%2BAGEeA%2BiedZtYfy%2BsSK%2BJm6V9CgheTUOoGkoKzc7TD0t5Uv9OE%2F7Etqi98xNl2FtmlMgBZs9FqmfKw%2BpB2E7NSOza1wxHG82owz5ifeunQ%2BOsY6HWPVT1NuAFFmj5TTac17UyvOemJMHIyUnYGKVPlUnXOVZ4iMIfkGZuVwOFIPsBpHT6J52MkhogLB2ykK%2FjuA4wn3hzsX%2B%2BGAqnkvpOiLbrL0H%2FXoE2mnPGmj1OSKwXAM7Faod4y%2Bf8M%2BO%2BPPJHU0z%2FgQoQ3RrjL9x5URnblTcuWmhKGyvDyU4X5zUgMcWjZDsCCMl3l4NblAi9AeEWlhhDaNbmss%2BcMtB%2FouDe4lLF0ufR7YCEnQZeUjsypu%2Bsn4fhevME84sIjXGupLYd36c1FMnP%2FeFlob%2FCQMJcnIDmZ7sztD6gIn3Bj3iE7UwIo6MA%2FTa%2Fn8T8yYiKBD0x9wypE0P%2FGqVxulleW8w8cKdxwY6pgE%2FxE%2B5hBY607kZ1Xr2ogJogLFdTA8Al5BZNehE6D5lVgl0H2mPvE6kFLN4WWRgJFU8MfcGIDqpnrjx7P6vpCUkNo0jXmXpa18YYVcIqwXzSqQyPc2cV9K%2FaYUcC92dO64JR58HT3fV2zK7wcPiTK03MwnSEkMOsXH3C68iJWzJ%2F01V%2F0YrkTkgh9DSmzO56AE8ukRivCVjMbo%2BZ3237EQ%2Fi8aT29Vw&X-Amz-Signature=559e8a39376ea026eff67e6492135fed92f331508e135b8c5152c3f18bc316e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
