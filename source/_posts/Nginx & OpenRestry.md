---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZFMJ3JY%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T090039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCIAD%2F9fbrX5xvXL6t%2FnNi9dOitg0%2B8Bvvru2uW9wj9cqqAiBe6hZnCyWPbvmuQP4lmAXPE6lZVCcHX1yStwn3alNcfCqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2a5FqZg%2BouAVgNxyKtwD8xBMn75Z4Cw5Eoo94k%2FKg3US82NCSQHYN6RMdcOHO2v3rhzBVpvXiVyv9hBMXw8bIrSzrdjnPk5%2BAsc4CaITOLXs25Pj7mrfpWuxMkUa8UdGclnLIlgnAPCoTVOIPq8TUCLphy%2Ft%2BSB8pEL9Kl60zXPB4LhOIGD92RbLiUAWep0G7nboJh3S2fTTyX11%2FNIfaHm4ZUkugjFd1zK83O%2F%2FuUnTzdAQqlF2GwrJFp9rIRsALwjurvI9a1J4fy8x8qC31tfCfjM2kdhhCvglWa5c4U8l9q27l20qssGctyGhniHTe5VUg1lzBH9JXpYyMo%2FvmAKFVjiuizPoAfLl13xY%2F3XaYDnEk4Dlb1%2BnCjAaAcCtmbfJhA9RDdTJeVNQAv8LfPjL3KghmocK6CSdkIBsf3GCH%2FmC%2FdNZjk6ShebhvE%2F%2BPfCTFEdtGqrJnZ0OVK%2BmrrPKQjy87vwaxCSoZXGyOml98eIVuCzCZclTXvU1Kvn9Ih3BganYhTeD8xRU%2F4%2F8eaxbVvuqkQaJeleeZJoHvSJirtdfj0AXt5RI93gXUDvK4Q9C7SeikhYK2SiN%2FVkEtuKCSVu3yTvHVy3yCi9ygEjLWNQmy7eTkwcZwWwQZCWgrmleDBqlddF2274whpHpxgY6pgGQPl7Cju%2FM0trpdw5ahaYKl8DJaIEsh6xouKAEbEgBbGOAiruGAD32n9fgWaQB1KceUrdOk%2BSB8%2FPuXOZzbk%2FA15w5BMg5SLjPDlK0i90zF0todYPyJHKL5nxmYfDMEvppuy5dU3ud0nqQR5r7Q%2BGYJCLi2UGPkYyEZWJA0P6jONVo2NJWmtQRUOGijF7bKMseIkiPHfIOrJuc3X2F36HonUah8In7&X-Amz-Signature=0adda34b0f2b84fc17646cb297e72867a58c1a961100a9ab2df8d4e13fbe11da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
