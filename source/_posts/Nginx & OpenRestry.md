---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664JX2H7HO%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIAdTTRSDqzavtnuKYm%2FKmAdfw6ul2ayrGcoo2XLUeD%2FbAiAVTpS1jUE4kTp4qoqev0MxG5jsZ8xewHOGKO1chLzisCqIBAjq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwRMy26Zw7%2FJn6rE9KtwDjJmCvqpjxSB4ngzYHPz6imY7kAeH9%2BtAOygA1G%2F4kyec5XFG%2F5Y8RfhbtpW%2BTsZxPYNHodSaQUDKq003Uh3WtKK40oizDReTLA7yaXF7wqg8k6VZ68psX95h6jHvRSYp%2Bbqd%2Fe%2FZ%2B4qFujS2O8n%2F3DgWlcK44rMYq8dRBgK0IIx6FdeTJ0kQhimxvngV5O4VHDaeiJ7hTwNKK%2F8HaoD0ZIUZHnjHhQySwjyfUgdPWQQfgFTmMLXjcTExJc%2B%2BkIcmDcVZS0x5reWp1%2FlpO5qIlBGG2%2FSxyOGvUVrPc2FxMLS5MUvzrJt2b2awV3760X%2F8fHORMWw5jKepLptP62MJsWlCMWsM5TErXq4qCqwuuP%2BAYkDvrcYJ6TbmCwbpqybS3tY0g6Q0KuwAus1iaZLIpuOeY4rEiIyBbWXlToFdxAWS51uYAPN5oJj6Zklh7zTdpzcVCIoccR8oob2066%2BbdJENowaLS6Yh%2FHCC9GV4exdovqkACY%2B3UirCx4n8YmueOYcuKhO0qrCWkyXR1oGRPjwa8PrXm0eNwS939I3R2mdo9ivFMYQRHIRB69UKiM4D2U%2BkhuKAGAYhCsmM8e5CiW2WV4VxACALg%2BHBV0Y9CNWovIy5H1MZhnq%2Fu0owxNX5yAY6pgFcu5WLi3XqzZpCwnI7vnRrMXfPVFffUBJ8NrF7UHWqzP4dHVI0XOaSvnUWjM0r6ry5XrxylirnQXPetAOziDSqqO4KduI5Y%2FkznTo1pyGp8ziOel0V7ZZEe4xpDbTI8FWTe1La%2FoevrGTQEOB9US9M4QwXnucx245wD3Dw%2F4Z1ptny6noSNsM4olpsZLyj7k5kS89xuRCwTDWFKkWFEhYNr4QeQyB7&X-Amz-Signature=f412a52c091a3fff90e82eddf91525eb2c2e8b42c373594828f209f2ee478424&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
