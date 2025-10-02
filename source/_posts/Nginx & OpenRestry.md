---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCPXTPB2%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2Bf6ufiP03%2B%2FCTZM%2F01lW%2BtkV4a3Kezj0gBefgdlR%2FhwIgc%2FKYfZr9fuwSEx10h3o3x3TRBRvOdboU80IvzKd2d%2B0q%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDJSn0ZO71%2Brvlj6rPircA3P2FmGl0XHyPYZFDlxlxt5LjmTpZqSfAq0FtatX3PfT8QiOifNoLCu9Bbb4qECMMRur11ZBBtRDLcj5UJrFZ0%2Be%2BuMZzOSotwqD85uRXtIkbG3E0PVSC9LtZjDqH6Xyxxiu5b%2B2xPnfIGKsNlb3WP9VOhQKsk7hd0FrG1LXKwTCuUBOujkRJTKJ4SWekMVWlug5Wf55dLQgUKtNC%2F7nujBMfvpoNEJvIBDrTGZmsyCONtB5d3Cnj3YEft0vJTkqCwx28x%2BDwycd7ZGbUUVoRdUI8feM%2BU7xtc26nzTIrRdhdEYgSSgeqZBAXJypNUn3Eawm51dsvytGVi6V%2FJce48oiJlM5OrLtPBOoePqS6X6rc6UMSdiWvHR89PsL078enESEuoigh0aLPii1lSYP8SAQVQhigkoFEEMfnmC3YODMzHZheHzY6vsa5%2FoR4729%2FyBZ2QtRzP9fy%2FQtkRk%2BiRl1xzjK0HFqig3MFwwlw27FIoD6OnQSHw6GoouYS6y85GRgg5jF%2FpJ4C7CmUWO1DydWzNCed3MiFvy1PotOuv2OrdGh1cYxHtmsU1EZOX2VeZDG60Es2G7H0K2inW7bllkCs0V%2BS%2FClQ39d8oOku7pD7AlW%2FA12uT637EA%2BMKKc98YGOqUBKWNo2NezRcd7x%2F3l29kLk5IeBz%2B9sqgil9mdHg7lWaYblE6SuorRfirC9cnLsCUp%2FO15cGALZ2G53k4dnToQm%2FL2Y1%2FjKZ%2BoqJKuGCdqlBN3%2BiL5kyWOdPE9aoTF0gExP1nghPmz0b6cyWI6m6rt0VBY6iWn%2BWUQLTEqY%2FEH9Oddva8nPFuNiHh1v7nb%2F4SQwPdq2v%2Bvbgy%2FfNlTvGRuDH3xITlT&X-Amz-Signature=f0dd9584be34c2cf4aa11db2f5a2edf148e083914713149afbcd424c64e3a35f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
