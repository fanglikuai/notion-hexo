---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UO66UESM%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQDk7G3%2BUoufCCfDkEXvL13PCL0UvYskAAHiEO0dCiRCqgIhAIEpgj3no9m4oDVwxw8ROhyR3q9E%2FCCQWjLKDeiNsMSoKogECI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwz%2BI1T7U%2FaRCJfWKAq3ANVRWXgw9dNKBPT1eyvVu28%2FhF4uJdPtRPCKOS0uncIkRvfXfBkJV3aEeAJpf%2FFtVU7IfC%2FM9eFbuFepFp7LWSLsqq7Pn5cJ4qi33AwivSRd%2F0TcXLJtv4WkNeEkFpSQJ5i%2FDVUdp%2BDi9BDTLA8vjb3KYM7CmT3Ocmy6SFE1LG3G4EgpmJ4mrFdXFLiI6sZcFHpw4062x3mh4UqwhZTlouAOYVQSh3bAnaM4dDBN3jt447lzhlJ1p4%2FqcGqZwc%2FxvQuoBvB7vSIIJv9Hu94JDQ%2FudHp3E1O9FejeUw2WlGacBTGAfMv%2F1hnI0K6V%2FGiEjK7N8ZN8CbNsXgxeDZkPEdCIc8cSv2K4%2BDeufVbDnv9dfSOLCi4%2FXBCreXnjLVhDBLgA86vU1kXaeamByLWGKAWH2HtjNAhYspRE8XH5GbFdev2e83JIB1Pv%2F%2BhOprJDyhRGpuKHcCHwDtf7LF%2Fi3syD6X6LblvocxEoeP0qThYj0j5Nw28%2FnJpafAGgkdinq5Won6WLQ5WyP6bT%2FyzqQMo%2FguN4c0PGMAhhaqJP3q4UFNKLHtP0wsVnWHSqNhNAE2fybHbeIcHzVFgZMFlsi9xcNqzB3pJY4C2UDmmeNZuIs0vfY%2B9Tu85kcm6szDC%2BdnGBjqkAX0jVr0C2wIPl7%2FYgELKFp6GlFr9vJyPyD5g9geNG4rSgJ6g8ql9VIJvYT2g0Dy1ElPKb9zKI9iXmWtu66Q0Gymhe3ZavQ8qCo60oKZAjCoA0Ei29IDD8lBg4v0ZfyxpTO5%2BhEsm3T3bg98HoJt94D5C24VeXgf7ezsgTZhkHi%2Fe%2FSOqKknwgL5mudK4FU4vTNusC1ST1xGW13uloBqgPatDVYkJ&X-Amz-Signature=7fed5b96da6e428331623322aaff51e88e095e9dcac224725d0b2c65a2837edb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
