---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6I3GURP%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIERwH779b58ErCYvauTKFqW8dSQL0ZmL09gZlfhINrnUAiEAnT%2Bt%2BlaIuaC6czNbRKpAqr2bk15goijsrR%2FBsTdh6jQqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGkN8%2FXs37OORz1tlSrcA5dEXsx%2BhmFKx7bX9H3E4ugjAKPCD2IQ96PTkCpIXF53mPIerANPQEaRzizGYgMNh1J72ojJLWf%2F7tYVakcy0BtOU6KSJb5WaZcG9jL%2BD%2FxgYD9W4vu6D22T1yenmlKY7%2FEt4rRatamojO11F9xQ%2B9P9m6JmNND81PBOPtnzV4CJmOLb1LEyl0DLACHN4I49yjtCn%2B4DnPUphyErSh7ulFflJMs7ewWZ0UBY1NlIIFlbdqiXwlkGrtKLzN9WHXtCh8Gp5e5SKbI2qhNFa%2BZaGcuiJG%2FMixFspy0jcTIywhu3RvoMtbUNA1xU9qCb57Fas3CivJoEELx067klc5xXwzIu3Od0FOffGmneul5YwPHu3tFNZyUtIgCwugpNy6mBk27%2FKhw2PBC6uqUCeCxsgyakLsGdcKWdxFz1Gf%2BrzCOCHkXbDBO9oVjtbGmMvUAyUms0ybyWrYQAUCyI4ZbM638FK5m34htjQNk8YIsGTK9o5ZxA2M7paupeLEogW2A5BTIq36cfG7jKkMvxdwrx6kjDznLqoMB44wALiV4ucl5mXj%2BHQnSWIbYK96vbngEi8sUrAqWanmNzcnxslYnTZqj0cj6%2BUXDAXr1Pa%2F%2FjBgHGH5zFcopb28S4qWrjMPzsl8cGOqUBCBf0EJIM%2FyTxmciZLuPEGGmQdCSVEHdGictw8JjUrO4SMDkGBeJMeIhYfDGXSl9JDfhNfws37kZ5v%2FXYwouww%2FwWOsutf7dBWRm%2FPkqpoXxqNe%2B7%2FSJdJdklZV1Y4BouiXHKlwMwLhO4JydFCl%2FEHfUThXFuOhc1CGUkPrm0W82VZSxtu2upGg1okyFVg2F1gEe1No7fcP2wIJ0bBC%2F5YwI6sLjr&X-Amz-Signature=ce072c827a1b0043a1115136ee8f3afacf0941e3f6e5ccbf4b6cab8a9f9d6a7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
