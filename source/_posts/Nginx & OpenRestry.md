---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5BAI7UB%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW239Wj97oH0RVlV7hHZg75VKLVCcnOIQOKMvnCPAwIQIhALFEu%2B4VOCICLRZvE7svFwxgX5ei2TrDKMC%2Fis3iRKzIKv8DCGIQABoMNjM3NDIzMTgzODA1IgzxWMWDQtI1Dj%2BGVKYq3AM5WS0K0Hfh8J372dbQgGAc6FuaGDh7wMnEJg34pwVLh0khHHq7O%2F8PCHVo7RMHHk462DcKg%2BKKTlI21QcevMiqbTnH9L4rEd%2BuajF9GY%2FZcOcqmhKGk48NznjopqAXT5TEb%2FxIhnbilQXsJnIjZJBgxRRjShCFjDGdcXaWk0QE1V1gTtZ%2Bx644sPs6DHvRPqtG3BEjOhOYHOxlzPdGXvdA1fcA7Z7xr6gU30ka2kaIOpk%2F1Di5HryxCdA1CUN96V2GQ4kwHNj9%2BP2x6PfWYw0hAFd%2FnaDkM6fxIAgH74K8PKyyY9YTsO%2BeBea9qJnVAEmrcLVTokKqS22CEsRogcRqO3G94FLtAoc5na5HAjw%2FDKJMzROqNFEuf%2BhgOp2TpWBaNIdbENFg%2FnvloyB6yNNYiftsIFxxEyflpA0dKUK8SvcDyHcl%2BEf3XY8DLlfmkuUhv5pXYemmP7gnrd35sFxXy3pm6BZfaFLDtRBXGCHdOuGIGuuLXHtfgBOEl1tWsTHs0lHfKZB6FYBLbjqwhvhtzH7pXKC5TiXf5FkbDK1t1XsxUkE%2BbM21wp%2BOibuURNjcCOKDYbNDbfMFWavd%2FL31q8pRwWp8a1Mt%2Fr2POcW%2FZd%2Fqf%2BsBPPWKJ4ZTnDDTs6PIBjqkAUPqFXhdLj3%2F1gDJvv0yzbfX2l1DpJy%2BAf4BerF0qNWEV1n30U9dgIWKQfY1sRGj7mDqao2CKLf6cKhT5YDTLQus1MPXpGMQveu8YzqiXjnM7JzArhC4yYTqcvsUN4zWB3gU%2BjI5uNriQ84wDlK0awYP9fa8xRlZyKQGOTPM7pdES1jXSbc0trdTWyioognZdZ1cscqlWNUQN7%2Fba%2F7sZ93SsGNo&X-Amz-Signature=9417cebaf5196c93d25895d9fd116b8851dd47294d54f17b6e585f1491271944&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
