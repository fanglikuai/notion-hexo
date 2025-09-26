---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHFLQDKF%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJGMEQCIDWDAGgJNz2xKi1SAlMqHpjdZPAq1%2BkF6OnnXP9EuEHhAiAsNO2NxsqB0B369crqr8u%2BAmqTxlQV4ZiobXN8gW91mSqIBAiO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM82RPoEl9ep%2Fjvg5xKtwDBPDf53OhoDEh14vih8ufvUcue%2Bte46eReWBgh3%2FNylcXgGmSzjPiPYw1hVSZ3UMbopWwjyDedZdSABGWiKuvC0FnjR0mLs5WxxyB5kV82kCGSXvorsyOhC%2FIPoYMLL7cq%2BeKAxKjZZzKa%2B3NT%2FxBk82LhS1hCh%2FleI4GfmFv1UhCwAVgCMc2%2FOXdXRmzmRPYC06pcyVpWPhMgeFLdzmmWXKXZ7HM0%2B%2B4qbh%2FogzxVf%2FiDRcdyCAzMQYtu9XCc2OF7PBO4FsYPBU7hQjJ4bNs81A8i2ran3pMunX6L296XH5RmTY9An4Fve4mpJEN8OjSnDaPLp1jO7dE%2BlvUT9tZlOZvpGyHdEMpT4%2BnOYQdyfguuJ%2FgLpGYMZ8HCG5X4EueN11AMiqCedyIl2yERiQ9S5UlssxaIvtmpLOJURzK8ZLVDoiecWH%2FQmMQtlN1N%2BWxgvcZcCPV%2B9cbFxJQV%2BDjy6Pi1d1pzNnX1XHh2Y4Pp5fqYmCWtrY9vWXeEhgE3OldWJFqit7FtLhDWR6KG6c0X6OftLUcZhB9Tu8PxlU8VY4IiJpG7eOZnbUqQTdgNkB9LekM1%2FQa7AG0RdJOSo9Mn6de009KIGOPOX%2BNfsYi5CZluzQxd15urYR%2F3FQw%2Bp3axgY6pgEGjB6wcroe0uTXZeUg290rzeRZpiMrfyVlDZ6VN6TjSu2tlIVmrTwYJFVzEO2R1y3Fxc8OYB2%2F%2F5GKPQ0pqdfMDZog4NLljTk4n%2FAUbxm6u70FGcYNhMIRjsUoePksbto4vyOuxbzOSYSn7phOTRKr%2Ft5Meu%2Fu1b2NCqV%2F7Q7bcPl3m%2Bh7uLQH4rb1xI%2FGqYtLw%2B%2FZggEcXiSBMMq9lztHn8VnHGlm&X-Amz-Signature=ca9e839631358b4f056eeb52b7e5a644467185f1697f3389867c3c6ec21d8131&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
