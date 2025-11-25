---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666DRCXFZN%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDJpH6YcO4FjN6NF7roX7lr5qun4b26VwOG7zsHaZFh1gIhAJn6%2FJFCVcZ1IiWOOm1qA1lt8MV9Tepr7K9wtF1%2BCNKuKv8DCHgQABoMNjM3NDIzMTgzODA1IgypieNxNHSlDbiVb8kq3ANU%2FG8DVc4QkpIJubD7DiQA2fqRzi%2FgWRnMgj%2BgAkGfXlDGELvGdrONh5FPhU3iUAb1p1963ZA8QW0HYX5rAXRMEjKtlEcAC6p9iVaxtUQ%2BfZI8SDzPSLwHpHiwt7SYs3bU6ESMe%2FKKuGMFN%2B3JhJIOqzpymw14f%2BPN9rXzY7bAM339T6dCkgx8n8LEhb%2BQPQTxnD0W1ZNI5Tk1oPGreTnyaeJXUr4a0FHl3MvE%2FE4j4rCAN1g6%2B%2BE4cwYLo8x1Yv7TeapY5XjCiCUsRj%2FjkZQusMB0fTMSz629ZYApL8r6hg7JyXDCzD%2BGrr2S3G8GhzceFSX9lYPsXnui1JoO79toZ%2BWmo%2Bl5YEigVqSSHCQoK2K4zTJIqtIzrwqGh3hKORsj9DyY%2B3xc%2B%2B2EUpOLE6351oBrd2BmHrGqHml4RMJCFOpTxamSN5H%2BksNuyE9JLfs%2BDpRnEM9qR%2F3G%2BNTBYO5IxSFfCAZaeyqrramwsXyc6EbGDibcWIeKRiZwet8CE3ZPz0mqhOH%2FZhEhzEW1vpCQHmjK3AlsPDDqwRlg4NTmvxiTsZjch7X%2Bmo5J3VM8Z7agKqjHzNVivt3Fm6Xv%2FNebH7Ncqf9SEALcy2EH0Z2igCgqfZEhh2d1TA%2FavTCA55jJBjqkAQUox9ditAF7muuqvQsN7LgofX%2BUMu9YGtPQHQuEubsfF45JH9pyaomCfTQXqGHxixNzT%2FQlTgRdDSO91Mh0qMUDZwuwVbirpaeR%2BfGKIXmLTSSPxmWaFTb6k93zeAL3dGLXEZidFL7ef01St4lyivKU4ZDLyfXGCUOveiLNDK2HVIafT2O0Mz41ZvORVQ1PQPoz%2BQ0VacSVRZuyFEtAiKO4qOGY&X-Amz-Signature=46ce5488bd0a5368942a3f86c6cb14930aab0e22ffa79aab6231fc9d8966c2dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
