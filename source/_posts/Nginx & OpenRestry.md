---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KBPP67B%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBuMeTg5MeEXn9plOY%2FYFQaDdx0pZ0uJyxzQisvoYv4bAiAxjtMy6Owkhh4EAHWPkgdqS5PGwpP5hTP7TpATx0VabSr%2FAwg4EAAaDDYzNzQyMzE4MzgwNSIMwFN5mqLwFZV3puf7KtwDA7PziXQSagJCHE5AoFnQ3queak4nOl0L%2FPMEeP6fBH8XNipK8F9MKnojtPsvABiZH%2BVdAkNW4THB4vH0oK7zOM2rg%2FUveI9sfjnzGz4t4sSpL1ceqi%2B6kjwAfu7qHnXUTw%2BlTF618Qe7yQHYxRdW4XHMNyy5AlExGdd%2BTdzCoR3nUFMFDQWL6u6%2FlBxFXf5Sx93fbIIrNODecm2Ru8g2dpjC7uHcOJ3qUUNIibOhPNzgT8JsUFfGxCXFsrERbel%2FBJV7c7D3l6kL20b3YJqIoOIcWLHmNplLHQ3XaFdVOzwe48cZ36KW75KpGS2I6iKl2f6ulX%2FKIwMd3fBTNcuKTZ4id9CWKB1kv5%2BkoV0LbbB9kc6xLkSA2CYO31sYxBgfBB5UDJkiMf18Xxg2zUyeOrQCzvOvC89fdvLidGyAqYi32AxKN0bK4tT3f3Kyur0OSJAZIShd2732CKEZeIcWkSeyr6ic9NX%2FLrNjqo3UN5aka5OWvHciV2pND1ydy2KxtdkyAKzY0fAibL%2BBD%2Bx5ljdVDiWtysq23uc%2Bl6g7reF6ybeqsYSrcSvexCy7kuaKbto%2BQKTQnkMxfcyH9WsZvjJYNQVFGSVO2DULUdClI%2Fegdf6O9j%2B60t2N1ssw0eqwxwY6pgHsV3s5zN%2Bv31tirvRrF5uttKjzhlmKb%2B0RNiSVgAoK7x8ya4GNLMG3Il%2F%2Folu6v6rma8siyammhbllbZl5Rydg%2BsDRm%2BuiMCenC9LaTHfaRNmAgp%2FtwPJQlMNZy%2B%2Bo7hWGZmQu2I9k10qaA2TCl4kc0oepSPEyZH%2BbG6XAqZlyvOgDqxwrvH2qirV%2BXUVkpxcv4oJFjOvCwt3rxmrflhRMs%2BJOuGtX&X-Amz-Signature=94387d1a9a4bd23a204c67209ca69e5ad71cedb17f5664fb8babb79e631ed353&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
