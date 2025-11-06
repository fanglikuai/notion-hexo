---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJLHNN4W%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiH%2BD%2FUdpcMkn5GebDH8RmXIXzSEJrEydAMX2ieZDoCgIhAPoMf8fmgo2DFrUHVHfZc%2Blvz1Ts8X4BeJ9dhOw92rowKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy4puKQNROmPvpcAAAq3AOfLj8c1XY%2BRJXcudAgHSaRYE%2F%2FEjEs2ELXRoazh0TewKJw%2BEUu9sWab1LbrAW5gTLlXfYtDBS3jVLTE7ggR6kfvwNV1bDuzXynUof%2FDatLNsaN6JiEtz7ZRlCP0yvl13sAzWYBF5ND5eucO043JFhISfkSqETXxSejodJlXgU0I%2FH3bssyCoSTW79BCsx2PgRvT8Hohta4seSV4HRWuT948Z9oet87lUFWqhqW7Qj3wgtSaHtXbt%2BIuaq%2BEaCWMwpRubDdVa0qMhaCB8iHq6%2FNHFQh8pCxRM8uoG1aOb2XTvuiiTdj7pcK1%2FUlAh5VzfMOORb7iwjIBCSO02ZelswZmZnHOQXSs35QuWShXmcFl%2BaaIvzv92XIoriWeav6v44BUB%2BaD7cPk5b443wghVK4%2FZ5gVwCPGaca2C1s6GLcPljebEvTIQL%2BGjRUkYe%2F6SD7Nf43StvVdwtYBIvYIiYHGLvGMzdpDlSBCw2t1MUzWPzxd7WUPWG9wKf88zilV5fWywZ1RH9IDIWrlN5Ejdt7KFEkZFshB7mhUYPbV7HWqIPoTOpj2F32dEfHqeCca7nMDtncz3%2FMp1p%2BPNIiRybv3XutU5e1ioijSuSKdTriGo7goPT1m9USXmXUWjCCurDIBjqkAcWWVFT33Fp1ApYi9sRS2Gr6LWJfwiMXmgaNUsOY6IGusL%2Fxp1PfZSwWFat5zv5u6gMfAt1advu%2BdrSWFP0%2FqXv1MbCwkSS03%2Bvs%2BIOvnFL5oNNr%2FVUt8dzB%2BwIzyIG44TcDSYEAdY4eBiKRBAEzfosFP9cYmHnDcFcCl17FOq45xUHlJ69AAOG4%2FYzOFJdXdsr%2F9%2FArL3NG%2F6LY%2F%2BTNV7cda1nK&X-Amz-Signature=4927997ec9169d336a35c7759e6b921ab40682bf7b8d947cf5a385c92001ffb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
