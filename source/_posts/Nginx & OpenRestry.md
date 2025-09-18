---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TAGJTY2W%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQDJhoKLWYG2ltvaIsrgoQt4bftJaIWWnaJkPd018bYB%2FgIhAOCQ%2FjQqONTY2d65x%2FI3c%2BA1kYWJO8hm%2BJB9D6lqsJI3KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwEb%2Fg%2Fk%2FAQX6Qg0T0q3AOcW5IdlGl%2F3fOOkqVeROG6xXmT3R5YJn4jdf95gvJhf3s0CLMTSM5x98bJ2UELQYp%2BtcGYb8mHjDC9F1zD5EN9l1pBfXrhx9hcFw5OX25OrpuBdwToe9YtaaWNBozAM32ZxwKpkpXMjHK42Gtq55YPyeTkML0q%2FPniRqCuK8XFfiUwIXqMPxn7uR7ia7kYhc0gzsUzChXcBAke7VF0MVlI5W55cFqRT0w1dWdY%2Bq%2FlzXj2HC2%2BU2DJ9%2FmzcB9auvD0HpyQko6E5Zu8ccb%2Bucayp5yz4kFH2EkTFRbUFSyVZlf7acfOeCXFGwDRFX6x5ztw6FbDD7mxRjY1GfhCjEqhaHfuScZ5dO86soOt5g0LGPBmaymcqzgbMETc2V03MfDzJQROf2nW0Vhycw7Y5OOdTVP8il%2Bf7mT%2B2zvhb5lD7QbN0nNdIBjtjmOd0nivXij2t%2Bvg87l%2Fl0lIXpPv2divK3S6kkDzZez9p1z6%2BtIGg6uRYwXRUhtCb8cE%2F4EKQ%2FkREqr6uWtx6zdGyNu6nbnjDOz9xqM5mTBEVBMX8tJ3eOx%2FO70J5oJSe%2B9PxJTy6nEVnSHz0RGZ%2BJUFj2OosYyFIgecvVph6gniyuswtJkVnZyHeT0sn%2FO91%2F84eDCgnLLGBjqkAWlxToYSnNJXO2oF8DTEy9aWcowillFYQ4eaxJTribWGwPSfG4nGLlxVigTaHOIg512aQezng0Z9GXRfslM%2FwnUHHsCRnp74DtTDj4sfwitW3UdlGB2pzfk3cETJnK2EuOVAHmeQk4hbQw70MispW0gQkF4hyA3Dw7AQBpDYOTueYo98F9bzJm0GQZi7xOi5WAUgdeuQCfLSnjE0YfsEsfOdrip%2F&X-Amz-Signature=2a33645eae3990cb1e795a172951e079ce3cd4742e7be64c6dda47effd7ad9e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
