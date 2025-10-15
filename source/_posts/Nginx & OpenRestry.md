---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHBFO3TW%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T190230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUJytxeoPOOuGI9vEeWbuLEMK5W7mEey4E0WbO589GsAIhAPO6M1zIoF4kPKs8YUFXuTxQPO1y9qss07JvV6o4LzspKv8DCHsQABoMNjM3NDIzMTgzODA1IgwmSB9oMKq9mfvAjHIq3AO20qJj1Yi27IlDrmFYJ1pVaOhtSrdE9F%2Biqwh%2Fz6J5ZxWXm%2FB%2FRF58nYrNV%2BXbS3vbw6RwNBBG2tle0YjlTduRQcVuqu3rlXItS6WwinUmNdxNnw3UwtZd2%2FbqZsgCrYkFTK0X18vXMTTMwIhOZxZeesatqD8gz0W15YdM9AmuQ0U6Fav1POG3fe2n5EIBPPFiK4%2FCq%2B01L%2FWFrVkBA%2FNQX3ThoqodaCRLDZNZdJz5BOUj649bK4BtbjprHFjOiiyUplJUQG393Df0JmckllOKJuq%2F6lfG1KdXXErnM4b9%2B6gzaMi6ZAuDB2Dwe36e%2BW%2BpsDU7a9ZeZfL17XELeAEujow98g65kD35GV15reLM9m6a7mScolNemPJal9kXpVpp8WVehecLGzwrFH%2F7uEQ0yyqYT1verOJuXmpPsoy14TD7AxSqVVpUShvOlVYU%2Fmvi0kXR7qnecc5f1CfIpsW42VeKymAD8NWpGMPGr6wB6WeKrTRiSsA8t44j0OfdxeNAA0FeJ7ue5lYwlkaMbHHnnHYygtBmcnE2qzCoYPLEPbL8D8rlJwxfiLXUXHSiy7LbVk7I9QSotjPdkNyUDrt4YeDLsXycpS3kjiKJCXuzYDmuQyArt7cqqzTzFjDYyr%2FHBjqkATDkrOQokpT6D5L4i8zLE1zx1c8e9KgvOSyL%2Fe8nZ4eBhcd9pLHN%2FNP5Ef%2BWk60PR3YhlUrvVLSZH%2FJGWYLC1ySg%2BID1Skf2izpt8GNq2XoBKFR776W0nY%2B7rfrO7eOKcPXcArM5rnzDySUbzKUtro0g7rxAgprsEBYJzzJaBIOH01aoYpGkFSxEvKBvH8ftpCOYg3ymCVuSsp3gFyKLwASD1hJt&X-Amz-Signature=b4dcd0aac26a15108f9711226ad4e9f907b7f9bd09cf4e4251cc2374e2bf132b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
