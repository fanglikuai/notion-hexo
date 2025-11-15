---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6AYBSPN%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEUBk07B6BDX%2BFhzOs8N2atZ%2FYNa5u4RygsElTahL%2Fw%2BAiBHDIYEmhuQHr%2Fci2oAQLyzXCoar3pfY2XzHTxZ5btqWyr%2FAwh4EAAaDDYzNzQyMzE4MzgwNSIM7wxbpCi%2F6ZXMXWqJKtwDuqhhRvdlfGRSTJ6TS3B0hQqt67IAjDEQ1X0R8RTtr7oFr7%2FBSWWVa%2BiEgVKkYCRz4mEqLADtNZVgK7F9rMOYvRm2fUvwPwTAajyBwwUQYEPWzXWeK7zGw83HdmA18Jn7Wa0Pv08tsbXDnHHiDo49BSgeMDtBJOzHPpSdO104WLE9voMnwrYihSpzpQxjegiop6bmEgRmMOlICUvXYLHAPl9x7JkzxKModOoxIo3PYReDSHVWNusH%2FWHZlOFJcmisZLNCpWFj1vj%2BMIW4L%2BKzklOTXbTdG0LC%2FR9QanBkzbBBXaxZ2tsZvwU6CTtJDBbOgh0Ve5H1NOB%2FxynmY9c9IeFRS2DgUNiahKCS9TZCfax25FT0ZkKkJ8WolD0KXylg9q9r4TiCYKaRWDDV5MdOjemFHvuOnsrtntqyXD24cmJ%2FOhi5Onpghnv7AND%2BDtMHOVRmhsSkhBdegUKC0mbdkwIUs3%2BGaYD5pM8dQj37QJWXkfdQqH63%2FZPB16eNTiGG3LX7F3Ggdy8283lXkhRSYUD6%2FB3n8kZ07HrGxNU5M%2BhbLxECe5kKpKJtpXgliT8VskAHzLcAbz%2F1sJbv8WQhm3Z9x5iuC2SY3ToQwTe7WX0WVZLma0rREk5F6kcwsL7gyAY6pgFbDX537sJrr881YWnGSosLz%2BOzx9kzu%2Flt5iLXC3ee73vrqMHDky3TXBHsjNmff79h5vKVbcjO8OqnWoY0WlevpV5HU9F%2F4hKikx3gGafwMF0aO81KJAh3vi7DDBCDt74Qqb0jzg%2B6iPyRVN8xwLx4JQ4Mb58a4Lo5ZhX4lCg8JO%2BOCYOS%2FtnF8RQ3y%2F8%2FdOO7dD3O%2FEsL1rrgyKK794ddTdFDVTFC&X-Amz-Signature=31be2b9e8e0d7bce946087929242b71743b9fdaa6825be150aaf1a2b48e238d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
