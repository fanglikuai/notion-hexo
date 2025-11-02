---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTCJ7YN4%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDFXc7STralWc4tOyf9i1i1GPcXi%2F1Sb1pKY9JgNLhQigIhAKfyksRoK7Y1Lefa%2FFP0i%2FJ5jLFunpAWqcQo7vMg7EqrKv8DCEQQABoMNjM3NDIzMTgzODA1Igy1tE54%2FcFRVzNp%2F74q3ANE7uiCOpRvnEFaBh18kFR5WTFVaTLNFsngXlxyVwynnJpOfEzqY92HTuUoJHEA%2BRd0lD9uCIqrjWj9cfFvjl4AtBjD6OaPPTLwKeYUBCnwptM5eo4c0WwNaflFaMgRpBir7YANezSHyL3%2BCujzKmgQya964%2B3Ly5aMPchuZXD8PfCdDVmaSW8%2BOAeHzTvDkFGxwxkCCUxXWpVDnorySW1avB8oqRsVQvHOPNlYlfkEI%2BYB9l7CNZ3CIKTB9rVGguwvQ2aVVrQoNaSA4xXqiMote47al1vwOeQ2ol3%2FMu6oDrRrSxAf92jwdmkls9i1OZE1M9%2BDXRtU0coZ%2F8mg8FEfqMC94%2FJiqEtvtZPIDBlmtMyX4oIg0pX7ZfhpCH1lBNjWtqYgYGeYZbhsUwMetKoJrLewkAF%2BoLr8jVDJKgr2Ml4CHqrRNLuBeuUztieeKcO2D6KmsvwauWvgptDooMzxUYP9C3hTAelvCldFsFNlt0a1tFwSeGIJpwGCUy7HwWhaA23EOrXSwt0GOxYOFKZIgUAV7zzw%2Bu4FSMBKVrx%2BzaA03Qq2BSDR1BUj%2B%2Bf9W%2FntAIa98D7Z9zZtQTAe1mMh%2FxJ7Juu0CpKDU16GkZTXqAkL00YUySt%2Bbw3EvzCI%2BZzIBjqkAefd5Gjowem1Lf%2Bcxl%2FS%2Bx%2BtpXrHlo5kR8ITvbb0ahzKWiO1%2Foep2FYE4vfbMxx%2BWm%2FbxBIrO1TQJiezeQHWy5S81HZ2NKUPXJjVmGReRUofYZhhjIlKe%2BHIsiFDihqK3MLYfI3o8ZWLPYfXIfRRnGeFU6%2Bttlyx2e6ArgwOoZENj2UFlR1UUuHVewhN7rXntzjQ9dhHxdooEZki%2FFZDF5VRnqLx&X-Amz-Signature=26c39fae3d014c8ef10501a973b3035f88a8f1bcd366e5b18ba641537b917d68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
