---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z74PLTBF%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBDzKdxaC0ZuLBsKltGi61zzCyZ7s681uEjIJT0uKHSrAiEA6ijtly%2Bu8L9noCXGFU68Uf9dU0LTCXQUhfX%2B0qOyvz4qiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHMjTv8AFgyivebSACrcA3uWNjTzBMCC0OLPssS%2FiKagyBQs13crB9qW0RAK%2BH850QMnSVFetCGGOlD%2FYUbGsHw6sTROL4CHi2IH%2BIYE4R7xpfsUvAv7h9LT89tZBrfQ6aVUvL4vT40fLnvx4r69p9bNZXh8X%2FZJy5YCMpoTQ6wSNngl4SJoCF%2FuhDS6B%2B2pjDgK2nUY%2FS6wersFFdAabZ%2FQ%2FkcNIlvz%2BOfVxUlNeCLMUHMYOejJ4OdFH4FGq8DdQqszwVtGbJ75GJlM0hSwK3%2BnFd%2BICfrZWtROvOVVbtwYhS1xerrFbBrNZbHbETDgLnJHVUpvPfl8u5tctHTVEw505ZPgHT5Y%2FMTG8wLxorTkchi%2F1T6%2F2ufFQyAUw59k6xhueZFiEDBnWuVbMy8SgQrCFH6lcVjghCKka2oJ0f%2BIag7jRvuk61QSrpMh9%2FP8NyGyYvE3UkfnqC%2BrwOG6ZkQGaL%2FEb3LpvSJ3R8uTgnGemVu6Krv6yDnpoHBwbmzZC3ik8r5jwV6I%2BhmVYSdew2BdW55n4quZitNJyqlJigLgfU0o54ziG7Ixfdyqik2g%2BLeaGpCRfuQL2ZjVlSbGLxdC7HMeAQqL%2FmNo%2FpdQeJHaA5TO4hWdUzsBWUP5tc%2BqdjLMtY2laoYynFOkMJXFuMgGOqUBmb0U3l1fD4M80X7Wa9tiaQgZS%2Btl30x3YunZ0Vp8JFjx5qXufSjWxacx2FJ22%2FNMYCmZscvoq2rU1z37Ds%2Fd1ky%2BcZqHhSJp16PjwZlZuly6o8UXMxVohqLnTc3rJDeAvmJ%2FLuV4%2Bdy%2FjRiyFNECia8Jn0%2BT5amiI6kyfwIZtJ3IMz0kLIFuyVbs4WOKMlcF3UTzI7DZM56cYIgIt%2FuXQGrSbfRv&X-Amz-Signature=d0543db9284faf939c4539135ecf299e0dd706d83083e21856088933d413c8b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
