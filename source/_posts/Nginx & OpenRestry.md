---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4QEBWQT%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T140116Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJHMEUCIHQtVf94nv970bAyTl%2Bh3A8Voe0A4Y1SGs4DQFLB2xxQAiEAwT4xE981adUGRbTTsmweL%2FUDrMpG5Nt4gsEWCFZAtbkq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDN6Rk62VPqAovQv9PyrcA7no%2BFjPpzQszJEliPg7e%2ByE1C5OJxgwak6tGChEkBzMVGOp1ZPQXr7cQ2D8gmDtl2QLRF4prEunmSMoQA7Zw1jqHjlgmYt3IeO51weX82up0pO405%2FvrRp%2BrhFNC78H5Z1%2F1uwiQQoHvX3GLCaVZf9h67YU7lLPt6gY7raEYa4gA6Nzd2gktrVO8GeJL8lz2Oda3EAUQsbCDjUs67tncDnr8zg4ybpZIOwEN%2BEmlajLvtx8YJsiV5QyMBxcQk1YhHLE0OmpkNKZbLrA1KF9XZByHXKVsELN0iqZoe5MUFCITU1vYZwVjd21zmFO7NffdbZswPK%2BuAeDEtdP3lkhHvalHIGkrAc%2BVB28tkoSVzrS4WPdHPFHDRqxjxgoFxZodilBUeCVNQGdMUYdtHpQud4badjW3%2FtlWBFSt8C6tntfGvC7mFuiSRfXVJhnYt1gWu04usGDCXGMPbBRMOkymtVeEwALGK7CSfQCwHhjsDtntORf%2BZ9kPJLXtUwoxsv51K6LDHOAWUx9ewV32PlWTBtmVASnSrPOowJZwHbp5Wbz5fhdUX%2Fqf%2FHOzlreo9rA%2FUS8ZXo%2FRodDwftXoFxAtSv5JvHB0dGBFEtZUQTvxvYv5tXTr98K4eJi2r%2BtMNCm48cGOqUBe17cak8Jg5jUbVOne54FhBA7Eff24foA8DYCm9o%2FcCR9MpchvvVYcgd0XGPUqKL4%2BM2YzJv5SUqaSOkDNoGBOq6XV7fOtMLS5JP2i9oUaSOnDVOfKZpWX6IEAnZqOhrjL%2Bg%2F04fThW9bDFCKd%2Fi5CbY5xjOZiDeEmknhjKFvqWYLvU9gdq7dfcmPGEdH8ldVRWiwFD3BE%2Bf1pVMnNbUHWRVJEdLr&X-Amz-Signature=22c231dc96544069dc1e9d98951774554e3a62f4c4bf77084e8ef389070ba141&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
