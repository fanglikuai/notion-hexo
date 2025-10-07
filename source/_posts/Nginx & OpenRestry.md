---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7MD2BYU%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T100053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQC70BHVX5UcYt4xld4Bq6gkdsyo%2BQksM1IWliEXTRFDUAIhAI8ysHFy2Q35yuww2%2BO6kcEMJkfjaAt9Hd7vf5eUVx89KogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxhnJksQuOkV8mPdJAq3AN%2F5IZ55Wxj%2BigJUN5Tm6tpbpFvyQLX%2FmYNr6ZeM7japDG6LL7c36DCnUc0%2F3612eupqRed1XnnJ6n0RG8WEvrysb%2FNuqHYxSHAAOWH1tvnwhGWqox%2Fb3SayztEmYQchKC5D3PIr%2BhlhMH7KZPMpWUqdPyR9g5KAmnYjyzomK3Jut31vHOUFhWwGQTb257KSIdJsWVuXFskE3ajldHL6fFI6Zhepb%2FdLeVEVGh1Li7D3NDTW4E97kDgUFIv7CBOsublADWpUgKYYUmT9SPAJuZkXWjLjlG1sW1I4FkMkpMpkI0niovAuUYcyhRJ6yhQDQbTVhIp6ByOQYUg9Dto2sdilXACz0FAwIzHugvkDpe5SjAEcDtg4ROHPLmHwuQo79vkMetZ%2Fv17nCCjUY2n1nE%2B1u%2BgDDlVAjkp%2FshHhwZxA5E2mScvDL9y3cAheYGrLW9D9c4uyUupkm32vrLj0tLQ%2BIvqDLvqn%2BMRpt3%2F%2FvCpCQfhuOvoVaKJGzLk4yYeJDB%2BKC%2BlhTl6CyJ2Vz09Ad2%2Fw5KZHxwnV%2FIVWjjwyYYZbtoPM7OrRy1Af6uKn4YQGgtI%2FMMB%2FsugqSIDoTqAyYsNVkAZ%2FpFLFfF9ODDr23eZUwDDaIHmRBrjikJfQTDXu5PHBjqkASaI%2FB0GYwFMY9FvfkJ32N%2BENxCyD5ZwZs6vUsctskwDzSlMyju%2BkFvNzxnJ243HERqUVDxqR%2F8%2BJm7Nxi5qjhe%2Bzrg5ML6mJVThnVbJd4g9%2FeARHvmOe7PqHPNoSTDnmSMGJqe10Au9JvKBR%2B7jaT3f2Z57e3ak2HYHH23R1aaMSTxINiU7SqUTzaLn6p41eRnLDZynLI4xLmquzF2RwbDajZVi&X-Amz-Signature=c32d58650673932e5f98e898e1809ac870e86ccef13da419367a9903b099a700&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
