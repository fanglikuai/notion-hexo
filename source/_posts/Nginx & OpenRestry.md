---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNVJCRH5%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T010102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH2HcaweTAijwL1ShJzw6DRM6Mm0iXYPK3LfQTi9uKVIAiA02yZsQHcHJxzUtZ3UfQOOatduHlT9kQMVx0yhGvf3Oyr%2FAwgiEAAaDDYzNzQyMzE4MzgwNSIM%2BtGsvurEC4x4RfMFKtwDhOOB1sQorNIMdchsFxojYqWPzi%2Bfq0QTuLRK8Ml8FplQmAOZv3iRp1OrOAKBIgvTQOdDq6hF2jxG%2Bgr9t1rqoWakGgwznh%2F6ycMEQovfrv21%2FiLhq2uw53isjoAjCXyfikhbIMMemlw1cG1U0zTLheKBnaTIGFPqt00fv5GFyKs4w1f6Tp1yQyP%2F4X7GVKxCW%2FNpLZHLRspPceIUp7tgpPcDSbDJhR7rQe9UC7B1J%2FqGYZVVo4TP%2BHb0wYNu8efWTA%2BF8J9BkSDNCkhSeljsIWH8H5UUVdmXxAuzITocRrTeMu43ZlEWK5JYXyeQNVuBbwKr4u39KjGPyI5jkfjHDzTmg5BgxViExsTW2n6tllmL1Wzy7d33aK%2FLkoKFsb0xJQoc7ADkZprGpq97dinm%2ByqBl%2FjaGKbxPYe7NIbUpfEirakCR%2BbR9yiaks1h%2FZME%2BHEdPrx5lE%2FGg%2Fs7%2BTSWsY%2BkOE7i2cin9cLGV%2BML3kIk6i3G6UcnSBZi0K%2FCEroJPQU%2Fe10xhaQCyKeYaX7t53%2BlEquxZefBQNu0Vqa3NQ9gFj8BLVnk7nOd4fj1etRQ9e%2FQpa9vPdYwvSDaMz7ftrYj8o%2BRJ9Vil0IuENLLCIh24wbxH2wGS3OPivEwn6%2FCxgY6pgFER39B2dHiT%2FArKxc4%2BNILMHGU8DYHUO3vT5ypO18A%2BeAf11NRouc6WeF%2Bjf8eEvu5aXSfIS1YXN%2BQ4Ym5rKZHVSp6KqS%2FtvvM3%2FRSzyfGoL9lKxgk5Gi%2BS6SqiPXJZ9rO3R%2FcFM%2BBE0SwUIw70oaxi38CSGQ%2BbegjudvD64LwimJtQoV%2FbOEA2BDjaT9jQJowFUCbj36SOT3Ad4GTcw2JjTnqr3yQ&X-Amz-Signature=8d68e3a54bbfbe33acd1ba2e8a56e3f2f7b228a5c444b201da0ad6976d20281a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
