---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBAQH72I%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQCdmZfCbi7yuOxHM9smp8tT6pICJ7D4vLJt%2BfFNWqT9dgIhALEP6bSWhWSWaRaM4YyqjU3oYkFU%2BdXONnW5vZkvwwScKogECLj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyaYQo3OGWfg973HEcq3AMxX4P12PpefoATQsfqfzVni7xjC2c9B3MZmUD%2BCtqPvCCrESw5w%2Bk95wikbL2hID0qkyWhUyFQi2q8FNhb%2F7CvjaiDLnrFGJAeculHIqB68Nrf6FxY4HxWR3VQ00Q3U5rwPahJQBH2OcvUK2ZiPAvX0wOkFc1GhiHCmdYWqigWtG0l%2B11BAh4J71uMImsrKP04LSQaso9WBE6POAmkBuPmeCPdijn9DxV98WKg1bkdHPVP2ya72VMgeyaSwTc1oGrFCSaaLb5T3j%2FpZR%2BQ9gUVmmrKOji2VQyBlS%2Bf%2F0DNV6WgHFZZaCrr8A2SKpmfi5DySbrmfy%2FSd%2FjTU3pZbwJ%2BZyeKwguyToJmtzkVa06izTGZJQdGvB13xfRYUnkO1amIQYbAIE%2BLfNavwscQtxSY0eVDpdnLSzwFnK4sB93QV0AhD4m0OhrqRkakAj7jsH5qJNKAkw4qWoQQSQ6FpQtpfqysYoovzyjeL3Og75xHFBkmtAvL3jVzN4goJLXAweSk2g0JJw%2BUEnkKRWq%2BUiL9A6AM%2Fujq0j3xK9Tkm0MXbCK%2FTntQL7PxFo8YQvYC5NS7F3%2BLZEPG%2Fk0m7yD6cm9WvrWmQNBbuM%2BRu4CLLYPDxRUAOtap2bWRkDEuHDD85MzHBjqkAV1djxi769IjT%2FA583bVWU2bVxlpGx%2FS7h0q5G0ogNM6WwrIvnSplym6J7vz%2Bmh6DyBm%2BZAAL6zasXpUoPSHGyFGVWXfapAtqDG2XAe5vwul1getybbDcLjePIfcZ%2Fp9YHR6uheo5qrvf22V8FUpTXWJzNkJvVL6%2FsIXD9T5%2FIRUYbr8us24GKh9a6R3acajMAlAaAeGN2t%2BcX99CQK9sTgcFqa0&X-Amz-Signature=f8652376ca5a11187da06e97e51701314619f382e82b6918e42f6f48b78aaf66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
