---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PV2Q7LM%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCGfcabprnaXWTE8WVMiS8BNnQkSXJSXXkdQ4pRCaE7LwIhAMnMgY2sujYoGNQV2nK8LXJVfPpDK1AukzCHIF%2B6vIZ%2FKogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwaW1k7OKDRdGM7Bv0q3ANqdBuaGGjJ7iyNJNnzxaD2y%2B3j3J6%2FMXtfTzWLpxlVItaEth%2Fi7GYVoECebd3iAwYrRwAxzf7kbeGfgFmlcGX4UIDrLgR5M17UexnG2O19jlb7oJLAvkmznHD7V311EgCQMtU9eRw3LwOOZ6qCz5lf2aWFxzXAnIe6UbDZHYITtMrfrjHOrIiBwuxPYal18f4Y5VJyrJDbAc09d4lB%2B1IWaUbMlbNY8qcN0uI5DYHslzX%2BsL17mD82R33Hl80MCGJljeUJ5Rh2%2B93fse6rGBPHiJqOA2av08dFdYyzqVbbi5wMjnSviGg2uX%2BInlf0UEqofDnxJXqpelqI2sIIpfaamD5wLnXdntxWsauGw%2BD2pmntKtQUOd%2FhWkY3PGDOiIr6S4QjnMXQXhKT6FhCBvoV2yf0io7M96Cn8w1zSQ2sqbGBN8ymtQK64TvuskpOe87kE49%2B10NefuJUYORXjQgnZITcfcg47BPkbeL26qN4JKsVB%2Bf0e%2FQrf8WApcpG3LKa9bLBMvQ2PXBRKsc4h2zXzArVuUct09It5kp4iPcrll2PTiDS9VGfiQ3MFxvXdNIzE7WVfWO4kfTlvYXdtaB%2BDLPtMLqDTgyo7AulgsTbKoh9Qm9qDE7zZ7mpNTD%2BmeLGBjqkAURZim0qBYh5l1LsK3GVG%2B76X3z%2Fgay9hsjhWOOuscLMCtLI3eeMCSDJOaREz6xFDV%2FIfNcuJHSXm36M2cojfgJ%2FFqsHDucHKyfgdqf51DJsk9xzGQnE%2FBQm1b167K49I2tCgaXtYmItue7cqDIWwN%2Bh5xBTOwf8UTyLkzBAshYt5oxtX1lBCa61m48FKG6RZlpqH1lGyqQV%2BdWe0zWhaCTDxXja&X-Amz-Signature=0de2bf281c97c1cdff912288077afb85face011b18e530145d251b23f55f2a82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
