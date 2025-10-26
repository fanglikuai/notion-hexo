---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZPUX4KG%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T220054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHNGtIIWIsKgqHH453edd3CxTBoJEAYvAgVoCryJfy%2FsAiBRO5G5j9u%2BPLAaV9I3zWJ6Dzc2%2FrZAKi07d6c9LG01QyqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMR%2FEKSPgE3v6zkqmOKtwD4Fy2pK%2BUC%2Fr8R1ErY1iIu%2F1P9fKe4TWs7UWeuuBEukUbmBJClDvvkHKsQHKmaG70Xr3USPrI7nEqUO6XjMO6GLG2g2qDd7%2FynKSlFQOcVwmFgme8Hq3wDo%2FYCAXfuaabn%2FHKuSw6Z5dJfeMyAoi6kzJtXO0OoGuc7tih2n5I8iOP1REbe6%2FsNPORLfUsj5wi%2BIMKIGI9HMOj%2BSuoFWwJlgxA8t9RsxrDKVozmrKJvQ2lE8mQEUC94Nf0S5MjHy6m3Tp0EU6mrB2b3wfn1sgrpCCr0InHtyL64Kxeqmb0EW%2F7GvLXyucKZjDggQOXcmpv9rJAWFIKaAvfWO1r8S7zNfejAYf%2FzDEQywHNNVmN5Q4ujR87XqKRR0sW9gXxCmOnP4pZcXYzGUlfcMrVJSyKtbP3pi%2BBGfX2dctsFy1Xix9LCh5qMPV%2Bo9atObTZbe%2Fxqw6gx1g5fuYb2KHxzZH112SFviUQjKlETIuOLqZtQY7%2FTo%2B%2Bw1uVOcaxoTyirEPD2fIhM4OLFN9rVJnghunSQqToDMgLRaPWGwkXHbjtHzA2NwJ0AD1wvjitjUqf8mDuRDfg1XDKBnMNhhc%2BhEGODhi9pRpedniGXAuZo9tedg49LHgc626bUdffPEYwg5D6xwY6pgGX%2F%2BsLmGy19MpLnfcCB9ZuEGeVgKzgrjC5yojZpqxJtJ%2F4MqhzR%2F2uzvsY9v3sEPWcg7MJoAZdL8pxLgh3z0pcRmRVyTeuCoTiISli%2FzvkIf3%2Fw3nnH69RxFdTBEpfAgE854rcv0WKDoFYkhCjmGXBXDWBXhVZmHY%2BeEr%2FnXI5xqh6ykT4bGpTsqW21xfZ5abdhO%2FJm1TC02zYUeqVbBS4EQmVdyMP&X-Amz-Signature=dbbaeae4fdf848d35c29ff32f9ca954dbab93ee3e99c649d290c5420efa067de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
