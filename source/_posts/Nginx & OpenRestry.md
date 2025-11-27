---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VXBMKJCB%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNI8mN4ZIpa44eyl6kWRdfDRTqzBo%2BNZ4BJmoXXiAreQIhANMs4lkbzFcPela10p9WmE7oi%2FDDQq%2B4gNfOxuAg8jhZKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz07uZ0A7orINFx5NIq3AMUT%2Bf0dLRYASCnw4ebzzb%2B%2FTw9TnP9h6SJu5Evj4XhL2Xmnu0jyI0U61xutQuXnlIJnsjRE6JZDGu%2Ber4%2Bi0YIbT5eyyFD5cl9gik71qjGZLpOTuFeASjFdMFcWGYjSOkE7MvRZQkXWI1W1DJl2HtjJg1kr2U5UduHqh8ejzZOjbIr9wJSLBogRhmcck2TZh6Ps67qQzE6WI4%2Bo5Ppwdfupj2Migc%2F7ZtnB3XG84L3dOUZQ730BZqEyzbdfw7VRIsAnlMiR2E2j3cCwMq9UNRG3pxtHePlVIKRrQJB62bkoeVGmS8cuc26UncJdRjbS5fT7aUYxaB7F3j3bEs%2BFDSxE0WDBQADUMDJBF%2BO%2Bj5dNTdEiWjhNI9Fob6mVjoVnhZGjemX2XUJLmC%2Bwun%2FZI5ldsUr1Pz%2BBkCoNHSojePeA8p7A5oYA%2BxIMfZnPa5YL2%2FAwVFFIyEhXpvw9S88V1ZF%2FdPUGSWW0HopHkbApkvirNvA2bIlYwzIPvjdO4L016Eca1cIEX3UEpBfCbIumg20H0D%2F%2FVzaSpa9ifK5EL49OfZcQAyoDOQDvczLBXUuX2vizc3RXWmD%2BW198Y7uhF9BFWHnqt6G9QdsGsmFho5a5IL9Q0%2B%2BYdWo8S3kcTDSup7JBjqkAVEN9PKLpQGueeOwBNsvqD1uecTJNQheq8seTfXohUwN5D1NCEztC3fo9FGuK2rVg2efoZjLdAkGU%2FhpjKEUsLrTKnV5%2B0pudh0zgD5%2FLmymgu9CXDgG5yimvgXU0RfvC8mm7n1H39H%2Bk9k7qezg84%2F5%2B%2BcnxEOyoGfUwr83RuaJjjeOAKYXfkJx8BIYskI86YmHVIOjRCBKtrxyFyUKOuTzGc9y&X-Amz-Signature=8e8f2388bb746fb045f7b63710a1bb4d0806b817eb77bc6ba99010f81a499efc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
