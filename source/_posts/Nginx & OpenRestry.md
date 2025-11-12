---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDHHP6XR%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJIMEYCIQC6r7evkeIUzedqR1Nnoei0XblccfcD19aX6CaBV8cPqAIhAKBz7SmbFEmlj%2BBwlvEwdR6cmWrneC9a8afN3xYidtnRKv8DCD0QABoMNjM3NDIzMTgzODA1Igxj59NF8jyGYwReNDIq3AOrwMJpUAvstZBv6WyXT6glV5o5UozVomO2BLKrlVcMnzZKaZNA%2Fp8PVG15PJoeUY79ryHGPN4M8jabZIAiLw%2FNe0X1a4h7RMsDUpUnC4%2F68OXrWvwi2ue19u52zqiWm7Hi16F4s4f2HeaHWKZjAFRdDQG%2FShxEi%2BT8tkvn2jpFmkyJBIDGLOjuWPwBgMKuXAXJFmr5dSg3LlKFz%2BWvH5%2Bxzc3dwVZcgvRoA%2Fro60pICvjf3v1mRrxfw33UGmi7wAJrNTxc9GP9U0DsSdBlGBRhdd6NiZXZMw90rogkdxqI7UdOB5YBOVwqGpMWv1Ii%2B30w%2F0fdQUyWMW5gOMTX2zK2HsRjYTsI2gShERbps9WsTyPjS0z11vG%2FlHPE0qtAfoemUyYF8KNJ1dxOOFSgY%2BL80HzRIRic2JWx9qTxelprtSwO%2Bx340W2rMKbB4vP1nFWBaGHzvbbms0WUWNVut7%2BEfQ%2B%2Fvo7T%2F5qola2L%2Fp827PgC7Fzt2Qn77SJM%2FVSnglpWPH0J6jdkxtQS4pzJU40Tr0wyq4Ya8yPUdYDHRyIAD2aZ1jqTFbVasNcfBtKy4%2B%2BtbAADq73BGlsr%2Fn5RUsWtnAYhm5ebWCzAL7VEvWVHicZ5WH9vhhtjs0po1TDnwdPIBjqkAaFIbkrlF%2F6kdny%2BGVbXopGZkBoOG8UD8%2BD05sE5AygLHCblxCQ6oeZleXBM%2BsA2UavsI2oTVOOB4%2BhO%2FpaBYBj5THMzG%2F3IvYQW9EErymwm69Iz%2B7%2FyPugJTVsaAKG0n9qFRLrHoj7F0jy3S4btq6rWdj1SMH03n3jMXZN4aY2MPDmpd0ZqhI%2FrFq3rRK%2BZung72B3MUtZfRt5zPzxjkALWSXjz&X-Amz-Signature=3de4f2f6795d1bc5c44ea7e9463f5e5474c3f3ea423cfebab387f806b0692369&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
