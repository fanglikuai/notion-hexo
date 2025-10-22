---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664N5SYXG5%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T030054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCICZkHplRQ%2FnWZNYJK8pcldwz08fmAuW2goPTP%2Ffy8AgSAiBTKfS1An5IUuZB28sLW5BCeWXgxQcvLpaiLFVFRlwCiyr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIMtG1jxBe4bsgdXp9pKtwD9Gee1%2BbrHGquxEy9mTpQgIBytyz14nOc6Rcnzntj8T7qKOPL3Iqlo2kDmB4rLvUG%2BaFuYm%2BzANvhpC1Pz74oZzyOM0y%2Bj%2BqaM%2F98H4vBr0SaMmoZauJukbQEUiStY25hqHP4syGfgBm2woduCCMIY5v6%2BGK2ubqsodrSymC2RZ4bGk0GNXdtlMwDDUAYiUzTfRCoC%2F1r%2FHnugg9c6uuPVCVqKhj6p6ycJ6iHHss%2FvO1ARo8OYFw77Oy3HcZ%2FVEV5JocYxl1QP3PegJMhqtnbnymoXsc442pDtQNC6Q4i4Oh54%2Fnv0wOKCwiKaAYMmXtoXAnc8MijMT6CrZss6Pqt4j47ydkxfNFkJOuXX7nuRzyc%2FQlJYDP1oE2eVppXMI0gn3SX6M0lHPEawRrZCq2GH9QDKmFPwJDVspxXtUZOGLKzFsO2XUwWYIhx3kf%2Fs5%2BnouNIp51VE7vwdO%2FVvDXMFfYbNbdS6KMyVm9do%2FN%2Bhj27OA756hBi86AndzKepb2pLAjRmw7YinX7V5pgoYRR8jqmOdypoeufAGsJjvG4w%2BiWXKaqU2f3wOtgqF%2F8O5pSOKxQ2%2BC%2BGozD87qSpXhx2eXST%2Ba%2B4EYzF%2BD3tbEfLlUqkckNOlQvtuD767YwzujgxwY6pgFGLOpaUVXnOSjHmgXD2on5vKx9RP4JSVxa406Fux2aE3RlOl%2FGptm7xecg1QA%2FlYYEbw2QE4zV4KCqGngpuic3VDtvEK8KrO%2BmIqNlJt6zVBPEnNopjFv%2Bu2SuPIKZVa%2BI%2BonGdLfcN9Mrorhil%2B4D2IQ1HRZhmje%2FXJ3TDxiP%2B%2Bu1GB6nht3B%2F9njF97E%2Bugv6lToWvW1kT6Dub2ZSkSIkTPuUuTv&X-Amz-Signature=a60ee67f850be77498b4a4f8a57cafc4e7eb37c74402eaa069200fa261275655&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
