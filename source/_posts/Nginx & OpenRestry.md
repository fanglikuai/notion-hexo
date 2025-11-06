---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466USBCQ5JT%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCzBe4gNq3d8aBtkpZTGc36xTc7bXSetHDOJ7hjCFvmNgIhANYknq9D%2FD6jTfiWSAyAjMm0gE5lWJ95CTbnQuMRXOH2KogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzsIpWXaJH9yfX9UGUq3AN%2FoOQBPCojYXOBHm%2B0Tfx62aqgi1Dje8K7GD7bjCXzij3RTsvyCclXEU2Eu2WSFMY%2BkiGMl%2BT5MPu%2B%2B3QzrZL8Q8e5vAQHUzV%2FHom1vfiy3pWWZpROpT4GzU6z1ntL6zoYltytvrV82Ok08soN3MBm1E8r%2BoYIzP1drtliopf6BSGr48yNGDw5sqNQuzj24maBHQM5wtHCOkmYhaqfdS%2B4apcTJcCqEW%2Bf%2B4tG81UEp0aUPSJhLuFAbNa3XH1e0jM0mSf4l048hfz7vTO0r0OgnSc9mWIQeYhftdM7LsLIXgsNut7Z%2BwNxL1ulNupk9Uk6XGoVTfTu9jA24FLjCstwSBuFlRvoaeOYorj2gkhmKC6QEycEzsEqolWzR4qREhNB2Jlue7GyBWwp9o1xOxYs3gDnXoaQPp64onTrMkxCSJpjIMklDi2IWnPiLeuPGvQfBgve76JfbTO2Z19MISBThyOSyjVy4WXg3Gxs8fHCsDqczuOocAddsyUHU4DEjeAWMFB49N18gxiArjOAffIwvnoRPCn3me47R%2BHw4PCJ1Fz8AuQ9GkMO8Ye6XbmUkvTQF%2FWRLlmZhdNLvrrL91WiwUjDJxnEPxW7qSEZhOr9v2jGOoDblbkR7e2twjCp5bHIBjqkAXvq6MbiwXQZp%2BxMvJV0mayjM5tLoWVm22mJkQVT%2Fxwp8Kzt2iuWay0NAmE0aC6ISlQkvMHWWP5m8KW35iStLCCyKs%2FyECIV0EUcq13ESd8MZQlU9xc36cqvv6PplDASI5ozPaYkXf16dyTaeiz97mJxM5TYQcjWLHhiagso23ywyIIXf8hF1TjSliihNBQxRA%2Bv%2Fr%2Fj0nkmgVNEyeqflMAixmvF&X-Amz-Signature=e81b21342ef7e24a8350bfa405efee5f37edea89180768e2da1d8b86424176af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
