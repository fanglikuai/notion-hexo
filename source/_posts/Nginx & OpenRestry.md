---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WMHKOTDD%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJGMEQCIDDWDHpLZD06skkISv6k%2BlWq3vzKZ6V3xCzjdR%2BmXmC0AiANb3%2Bdgz1mjVy3Gj9lxg72UruoDjFqmnB0ECWE34Edpyr%2FAwgYEAAaDDYzNzQyMzE4MzgwNSIM3lrks6wzOAjhNG7KKtwDkM%2F24IVkGtSWztg1BGv%2FlQawgWTTGZw3%2BhhotrXLkF%2B%2FsK1FeaG6hcFmQNNpyE%2BXRm16P2t04t6Gk9TLmprlrINPHcYjFwv5akZgO8MEIBZKNoANM3PpSbFvY5soAEoSXlCkuQeHLc3XIO9HPeBpe5wGiS%2Flkn39m21XcQ6gnMw1zpYanB3g2h7Ad6YDyRPLKUg5Tvqoor6nSmTUfDWkKc3T3QwqL9Mx3TrOShseBf%2B%2BLapGbYaepN7VjehGcPlihLhB0fshmz%2FifOPsLF6LaXKEf%2FObjvdLMtsXbc5Cst%2FcbhrYmP0eDkzbUYJtvzrryTjElo4yF7LBCQBgsDlru7bqaQVbzmuZH1iDU7J6l0%2FNKOqsxGSlk6vt5Sx0vzpw%2B0d9YDUBMtrDecyA2KZk8GOsU5hEg8t2J3PpIM2z2tlLUlyqUt56m%2BFXe41nfLoxv02Y%2F1j1kfu2aGqdWWSDTbrymCR1opd5n%2B6Qwty9rPiWTb6cEVUlY6Q%2B0OYsjJp9jBXkw9ZKBaXmxU2S9wVmjwSVRst9tbtT3J4htQPx34Z7ESCwcxxvBhhzmIH5%2FLyC%2FVVTMsPpeo2%2F7%2Fx9l6QiU2xiSWKAyq3IjMIo8aVNngJEPGbYeburMFZI85sw9%2F%2F0xgY6pgEnjxiVyukB4Lsuc12tZRNpIIC9ZASUe4CaNid%2BclOkBoqfLM%2F4CoEs3whGd3%2BqBR%2Ftx635AdVpmH2kxoQ6QXR2%2F53gsbK98FybrOgHeD%2BRB7IWhVrwGyABZ1KoF4%2BbNjwK5O%2FZw5bdzyZ2HybXnLB6M6EqVYrF62M5LiebquHfsD9YGjFGaib69Ul2BMZSSWPm9G16W%2BuNhCsxn57KBThE4R%2B9c%2Fk1&X-Amz-Signature=7bb180cb675c251ffc1974e65e4ddc20448e79b2521eef9f83d2f9d03ae61dd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
