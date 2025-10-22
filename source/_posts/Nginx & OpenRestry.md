---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPJL47F3%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T000055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQC4CqZB9Pl9RR4fLgiN64dXgXZML2BqWqRJRjtWNN5dfQIhAPqvbL1JVRJ4A7mmYvTQbzp7hHwJU9qYHXUO7kggSzB0Kv8DCCEQABoMNjM3NDIzMTgzODA1IgyR6Kk5FNVJKKxSuGMq3ANsDJ%2FgdQ0hmhYyD99OZlpTCtxZExlCyny0KowFbUxlNFUSZDM1KBp1iTxVC5LvZlD5fSgulik%2F%2F8puy2qi7maOCS2l94JTiDlFBY%2FJ31tCpo1V27h3GU273zJw95OvR5TSDeR2sVo%2FF3G5ob%2BMj6lCeyGGTQuOiJDoYhEss2VIkbOSUGDSlRJqVHBExjo42ERe4N4%2FpgAe05qtUcsJxBxNTo0c0GYzdFx4naKuautb7DYBW98A2EZ2FNLpwknMMoOlLrrextw%2Bjo4Mu%2FsphOMEEvgGb1EIiqgNhMvfYKmB2cT8wFaUX6RDzv51ylOiPeDkpEQP4LAB5PurDY2xLhSH1bgcOl5nzzpehH6zL%2F0s1Zcv5VBb7%2FBEC9Z8p3vNbIkAEQlb177iFzCALnEM5nMcJilMMn8%2F6JAq%2FlxxvzYakNFEhfeG8eqfBYMiz%2BhN%2BSsveS6rFxt6fJBOxu8yuCqEdBcg7iXadwSKGqZwptE1RKJgPKmUHdqsN38S%2FUv%2FUyVFABSC2HqYVWD9iE%2BuIYNuGrcBfWQ53%2FClOxSEw%2FoqOAkmhBzGD7jeIfS2FfnVFxlqbj50nS%2FDI%2F0wYrgdXynasTr2KS6wFUD9tJ4tQ%2FpBOsZpHHD4mNWmm4nI%2FzDKt%2BDHBjqkAWXmGPZ2lRcRXJwIDawTNH536qWMEeOpIxa2LiTGtee2yFaQ8FMeUTX%2FZutI5PMrsvDvCOlsCun25XMrzLA4rJUbS%2B6grsWDev6eOog9qiKVq2AGsb%2Bujtso9j3g9vMd3%2BYf0ty3xcIWqRtM9WAo5ccLkN56Z3VtIpBI1e2EqIrrc9wBk8egRaeOKr8tYMOy29WXE4IUFuI6QyItnAX3e12Am22t&X-Amz-Signature=b57a8a5cd88477699a58700e092164253d7665e17784a0680711418e489c0e96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
