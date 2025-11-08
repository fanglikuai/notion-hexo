---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVGPEFQB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T010128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDWSnm50rBvKvkemt0uoWuGflr0F%2BOzxitSyTFP4J%2F6ZQIhAOtJGrHxcHXeDcTdlT%2BaiQBUJekYa0QMy8rv1IEfF5AkKogECMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtDgnWsFEaOEPgXK4q3APWhEtIF4r%2B97w7kwjegdneIRr62MULLwGfwIta4wxf8iLyToeNTqqOKqbxZJdJJEwfZUyVXcqdg3rtmodIILBt7JezOf3Ak2hmL2p852p76oTVlIJsQO58%2BrbO0ROJfiO6bBK9NoEo%2BeDJGYKv5CHNPNpEVN7gmgQ90pQntYSccJ7YQfjExwq1jmJkwjJaxaS4QXnnATXJyg2HTi66dXJf6xOtm5%2BxHVU5nNpkkX00RmJHus4KTzfPoxS6l15wt3Cvp%2BhNrm2Xhs6BzjRT%2Fkk%2FxCDH669qfTAqN91jqEdB%2FcxgFHtO31UMiOiKZQlmka1oVlp24HUzPDHIE4FghvRnN8I9myyHwqNwSrm%2BwIdy5%2FYhUsZZzMl5%2BiRbuoBtPPHPH%2F9ML%2F1B6Q16JdCq4mz2XdiqECDjXXMVxaucIR32XS6KriDsSfHg2ZO6%2BtmyKrG4UZfHdpCLNhH53jCWb53Db4SxN%2F6whFo1BbRlAGE3Y44wewIPMTWcflE8cOCpyk4Btru0V8aujOn1nhZ%2B0E0l%2Fs2A3cXCfTmDwGjVgXE6Pv29wRDZZ1GO9lC%2F%2B6fGCVeJQvLVnawZAanc9ZR4lYgaU43d0TUYmeqbPrdXJd1uhFpuuq3Q3hg2ldFWTzCcm7rIBjqkAR%2FSpXWt974c6xS%2F2eTVy9LWmlkPwO5Z%2Fx6%2BBnKXPJjgMLffl3TW1epdjvzjlW2JSCly23aLXst3aRP1qEvq%2BHIdOCx%2BDYOAAPIP5DLWU5RrhM3RtNMgnebnu35FKy4cQbM%2BFAGS86RE3%2FzeLcO1qmJ00oFrLLDTCohKDIPToTUtnWYdVbpGj9NHJLnjgi5fqT6VIruDKPA2jWUJa1tEXtSNySv7&X-Amz-Signature=9d8c99781ff6f46ebaaf7389b42c89c72d1b33d775eb6ab9b99feb9b9bb498fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
