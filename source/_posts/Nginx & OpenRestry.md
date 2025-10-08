---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VIOEXYXD%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJGMEQCIGo5GcOlvcpycU0bwpmqmjFLID7GoVf4XZUVKOfKjepnAiA362QYBPZ7VDF1dmGn7O69wE4CzT%2FLTDd37XTE0%2FbgUyqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGx2i3V74isyz%2FwR%2BKtwDziW9samd88eJPLlR%2FqSvmXoUXlsKhVbu6gcQvM8gURoDYxeE%2BJLWGa15qnKiOLy%2FroYKj9WZh1rH6c%2BqjqzdTZoukKmFQ0VofwOh1omWdO3xbXGIfy%2FwffXWcD8%2BYqyT8oMYfaaZSTtO%2B8p8GGPZgYyWBuhjHYCjjMFlN0qjmDtyJvXbU9ru7hn5Xc%2FJ1bITxuEXBZcQTOm48Z77RSQnZT2wTFxKCrp3r%2F9QD0RhJqIT%2Bfd5oHRmLd3wJ%2BEEf6b6yjJ2orwLjRB0j4%2FD6FXlklluLCq%2FJLeCFZf%2F7ZipqIOgtFWrkDiuoKoAfKYEu4FNkWGvz7xNDQ2qgz5ZXFTJ52a5bJxMr6%2FpHN8PglSSDSZeoxY7IVERuI74DOmBBQEKHMshk4pZEUg0cul%2BZJp4gEwszkPU6UHR4%2BXo6F5DpGYEGoXrATOhe9sTiuaa%2BOS3pfVEIfPM34tSGvYrF6FBSQt1zDd%2BIm8zw3En6Q243S22Tz%2FT6r8lHQIHtTpP8ZFESdJGjxpUrxsBjGmlgMZ54HPEOp2JGZQgaSo17rFWasrbDaO0%2BJnEbwFntKBjMiD02yHOXuWnOl%2F6K%2FMC9wzrY0bf%2BU%2FFiqovq5Z4Nu3iwuZ8iBujVW1mqUHFg5Uw3YCbxwY6pgHEOYkju01baLXMC3toEhX5VushjcSz4sccEKpJqT3qor%2FJyfe6jdvAqlRv7e6dcal1HvM3PPg9WMFQ4IWxNARj2VFggAXKpDsKxOn%2BU2%2BEsX0bv5wt%2BReZBYtfWCI2lG%2FacHaBho8iuQpxGcfAtcsjKUUfmDIDQt0nRRmSodBgUxygVPMkVMhB2InZHngD4nyAPz6oKbFt0MTJ7kKhUzDvYcAqM8ZH&X-Amz-Signature=48a8cf094e72824ec3253ed85b230c9ccda70f060008101a141e62932a89150f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
