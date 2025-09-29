---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ZAOOVG2%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJIMEYCIQCG5lLmMBIVzdDLFEiV1VTXTGt87%2B9knWgUYcHhOGeUnAIhAJepOpvUBudsZ5ygf%2FLyupGYSCBa8cD3BXUPlzncze%2B2KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw6l0aQn7rSXx43fJMq3AMoHlFe0Bw7bVdsnBIvBamCkbMJo3oKMFRuZj1sa1lPok99zWox2Kk7U0zgRmc6U27mLWRFxvbWLyjBAmE8opFQhXpHeUSFXtUrdJj0XHvEHDr28XHI4a0SxRArmwTslOeYT%2F6f%2BVZhv%2BDfYGDxeXhfy1DuZ5Z6gdkUau4jpdzQ8Mrb2GOGQNY%2F6e0vXCWLIH%2Fvsort1mNFPJ2O71z9JMN9918Cy7hFU%2FAlUY09SPavI9jpvCV3PG8ZgHx2HiPLRJUWGLWv1NNho58kTNzPWj9wMLxQKFG6W%2FzLaXCzA%2F1AwtNBw9AHZU98oueMyUHT7oGDW4Nce7LnQbGJRxMDtSqMvVWLBf1oc7tLzWUtY3ajBg4fir5EfgMzM3Uyy538fX1OkyyO9D1wSgYLjvbP0TnDCWjsfwdnHwjSQo7TheVhnqNchKRGTUGxRss7wROZjRZybkQgDe9oyoN65Ym7OuRYtycQLcdfVnvageoQK%2BMw%2BHM%2F5%2FDejdnz9svllxtSb2wG1eU4LySJx8sGag326LfPRg1N3gdvVeVNZxUraZa4SqIOTmSVeOYSlwfH9XoKhykiEXdW20VZPqhSFxuoTfXXXGqK6mZZAOV8QUPHiP%2BSZWk5Lk%2FKzyUxGAY%2FRTChgufGBjqkAcAjxtT9JrTZMZwSWzL7vcrsKEru5YWe9UmNSNrayje3CzgcwaarVHYaJWQxUiP4PRuIo4UGUVf1W%2Fy1pE7JTO1ruLFva0hROik7IOzJcMBYot8PdtUEqPXdCY77Vqo85vFwEhhBwY3PmC0gSokf187s8SCfwzw4YDUcUroc%2Bkd%2BxF%2Be5ndafg%2FhQKd1NATnWo8GdpPjhez9ALehGMEUyTZO7tn1&X-Amz-Signature=20af55705312d29a35f2978c3aafd4723c201625fa4874c1e266aacae2224d7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
