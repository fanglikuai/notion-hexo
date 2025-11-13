---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DBIVIOE%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbmphMPNSBqYUlmC0HbrwLcQG3YfHjICm3WTWRErn5IAiEAlWgY71Ao7flrmiLslSdbVXeb9BobJ1kl5iQSdcM%2F4EUq%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDM3Z9%2BSj4v2ajNFVyyrcAyx1CS0qR8h4v4GgDttPwx41PumZ7LmrCAL6LKAHmLyMRCIkhR0D4kTTUR0iXBYKTy5iBrLevWHXxYQ1oUVPpTIfNQ7SU3V%2BXDgNqRBKc4HOTQp8gSGLIL3OwvF%2FIeqiSxi9jlz54nOAE8NlAQTIq7mLaeAc1NZoJKX9ukFvNyft6t3amesF%2BIeCxag8L%2BHdDM2hyNvQd14rLTALxTbe75h1RwtT5R9%2F0Nmj9AWYjmO2gzCGgVcDx2LbOM8Yg4YuMiPoSiVKw%2FR%2BHDBQZGaBugh%2FELchyvANBnhNclrM%2FyX%2BRba3IHgiyIlhYunLCOgJGiKastV%2BknJaSZiqUp%2FBiQYMT0F%2Fltq7wXg92v8k3eVGDwzAkkBVa5yEAs%2FtFmgrQdUpj5Ye7ePtUADmqd8RDskBhR7VoURKOR%2B38aR%2FEywDShkogjTfXMaoSW8yfPkRrJmgd%2FObmQG83Z5AAWXbmfHPOtLQj%2BIwL0pxeAd%2B%2BOuehY6WT9f4ZD1UgEM%2BOGh6HXExaKp9%2BY1zkR%2BGVzCeQuwflBdVFh1ycofHmYar4B8iDBrxxv6VzxeWKVgDqDzItZk2TD1A%2B5%2B61GMDxn0ggD1MSDrbfy%2BUJ7BCEelIk2aBD5UP565IU2MVUUJSMLrf1sgGOqUB9nEg77SbeQg%2BhCX0amcHERsUyzkt5%2Fq40jnrwRjRbTmWEuMx%2Fsbvgv8xnqo3mBMqvNlc0kxa8CozAGs%2FkE8y0VDGbUUgxiJgzoXfuSaJNT0WQ1t%2B%2FFe597HhX9U6bxwU5m8rK%2BuZNMjnt6OJh%2FxzCtALNdlCnehigYWiAtw1W93BPqry9tHh%2FZHyEDk%2BcQAZ7m4KNlKcJXKHxQ7JF6VXFxEnHchG&X-Amz-Signature=122901ae9c58e26b9f0a8db475f43c13686f9fb6bffe8d6a31784a48eb0ec88e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
