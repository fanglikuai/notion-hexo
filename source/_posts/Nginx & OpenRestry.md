---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YI5DELET%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQC4Li5frRcxgz9BCEPo52qKc%2Bmy2AVzPV7iilFtjT9EPAIhAPvYV%2FYvElmUY7REBgK7W34ys6qnNQmHQEy04Pdb6r2xKv8DCBIQABoMNjM3NDIzMTgzODA1IgxHOe7qF%2BhtoazcEgoq3AO3xB%2BXfsT0GuYKP8SVvDzFKQ5iQ6WbhMDl96dgG5aRo8MtjBNvPltH36kKqRRjJcum6BOXKyamSlCD1UWZeLEo%2Bi9%2F6jmJyOL6jPBb5O2oadZFx8fyZpMyxAQUCKpK1Ab8fqfE1bcUYugptPdUq6HigRQN6860Rb0CsgiX3pNvscKO1UiTTwjGVUkaOoJJ7bgW7WaKQI2%2BrrVH4BdN2z6eDj6jfOQSD8v8weBScdTJS0RlBx3C6jIJO0tcQOyAVWE31G%2FarSmCzetmVWcqoAwEdrrnV813hODlWqdquy%2BUB%2BLFUVA%2BBap8RG3qUOzC1ZLxNOYvlAnVpk3Ku9Hhz%2BxHn3Wwhcweebs5VqjhVrKR49A8gozkvUTE9Q3K%2FEcEybGhGVouKpSOR314%2FY%2BJdB0rXp4U3q9aKw1sXnNz%2Fxo3pmF1o0qGiMYWdqSLlMZ%2BAhtMsVkvraN9i6BrvRrYi3a8gBCQVHEQc6CY2AvlSReHDKuow8mCFgAG%2FRZ%2FrLmOJK%2FxvvIbwox2DjS2inu0XUwiGRGfzg%2Bk0BKPsckuvQT%2FhapkScXQjl73g%2FxDprx5Uso%2BQk2%2F02Bf%2FSPg4zgVr4JKmwZPwO4bT%2Fm2HpKQDDgSqyxPCsoEH0FBAZv39zC2h8rIBjqkAdDUzgMXwpRaKkjdxj74CN4O8Bs6sDn2l5M1xNJ9L9DKSAggh%2Bniuk26VFzCC6pXn%2FKSgEnXzfArsvqu8fdQTygt7CPtSLKtv8WSZBO4E4H6AZSeK8k2RxKsk6uZ%2FajYYt40SREHJvMc1T4mOp9IRWwJU55WR5m6MEjtnt4teCb1LHlEhBmHuaqE%2Fw5ACYL04iient19DZ4c9YSTKX81WkryHMcJ&X-Amz-Signature=b0a3a8812d971c04c5dd7571c114e81489f968b94563f0a1cbdb522e538ac7f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
