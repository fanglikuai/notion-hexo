---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZH7EXCCD%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIQDE281A611EWtTkfJKPLKk4GYznwoSw9811mXdLckd9RQIgTP5G9pqUf1ujSNGun5kjAuLYIFRwdqKTAcMEwxcpYu0qiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBjvfAhfD%2B3udsJwnSrcA36HIO8g6gc6lmSkH7zcIgKMtHFR1w5TsfFsk80sYxn1hZ%2F4R4Qzns62%2F3ZmHZBVHiOSbZEmDX4xfYgxYBZOyo3AxRpf8TnVs4R3FdzqD982hVLG2Bkup2eHzq8zCidKM9yKuk2hHwZkf9OqG6Jn5%2BkKlz4QiFo8sCFQXpzT952%2FTImblH0E7M7wToaVJlhbsIvzIBClbhg9zW1BnvLSWt8CZ8hg%2BHSFeVCy2DTs40H41nRO5GSfgQ%2BAZfV6kwvq2ESij3fasscrXL1JWFsSeAFrlkcAmz4EN%2F4kmjfNxIbB2W%2FCu%2FEuisfQ%2FFNgcerVMBEGMeLN3StGehgi%2FJFsBQmbBqm4PuRDRqq5L9Qx2571LnNCRqkC7NYxlYz2Hb0Wg56tZJLE99TsSdrWYtqfOm05gV8pelOEP8QXkH7p%2FV5mYf1F6pxsbzG6NPRgsyF82HgkZkWgSPwGMJv06KrdQbVdH9sAL%2BYzKNQyfufbJ1e3liAeV%2BnKQvMrgSoDszDKCZ1f3uDCYZB7F8NStyzO48UqfiC%2BgBM4j3%2FqrbN6vS9%2BQZSJ%2FD%2FdjVw6jIVbqcCXy6ZbRVPsAlnargjjtANsDQtEiGSQLxU5ZcOoP%2Fa9R%2FvyJjdgi0KLXkcqxVX2MJTFqcYGOqUB0KXZUowW8LHfgKqKuqhwPh803MLOCZ0VP5qs8igb69jzBVv9%2ByiGrTKsva7NZJ0IiILm3r7C22bY%2B1INvuFFvFIdNBGBHwZp2yIyiB4rw%2BmsBqpMJVJHiTWuphpDrLOPKTtlyLL6szMcu97yjbn5w5Ww3nmT55SYrhXPKQ2Sk8nSKVaynWZSopisrP1AAhc%2B38cJRvYBKHzOElPGjXBS4K9QfeVS&X-Amz-Signature=5b429d4466178d855480822ac22e078879c6c43c3a05625524cb2e9f6ddb8b88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
