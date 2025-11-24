---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBYUASQX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T100058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaVX%2Fi7fkxofto%2F99%2F7CCaA22rZ%2BT3g51ZrUlcYPePOgIgMkfjn%2FgaTrEhwwAI4Ww2mAGEYwITQkuHj4ofwEPWDF8q%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDLbWDJaL21ptxgSsvircA3HhALIq4SwrENfQCCpN69k3f4DVN%2F2ItvABQJwAGQ5zeUejIHN4DRLeDcJT75siaR3%2BsN%2FgIeT9HcEYAAOXrTwWB8heBJgxAGM0Q4mfj7w5tCAXYXPMzDYKXWTu%2F9o%2BFDFQoKMILhI1HpiuKG5X26i%2BzlpjaIq%2B7406CWgjI7W2iPbdSLlUlrbLAv%2FevRdt6C55nmB6uMWxldWQg3MB0EP%2BFwkEMecprmCgW5PLSGEMf1MZTQ6%2BsU37gr1PFyoqNTYvHZ4PUUwqGo09gqTO2V7FxOrPtgeDPDSNaa4RBdL9msZGlzQ%2FuaEEoy%2F78%2FjKsKJJBVgbW3z09WoAqvkEh81j7faoaLqGZs%2FuzyLS8Rn2kCpurutgtwLrJYZjYNSeqZ1mqkHuFA2B3I3yhgwXnOb3AvAtg6ZisLP54GVkUCUXBVmDfKml%2BjUYcbGoj%2BN0fQECzlyxdZNuFC%2FRtAFoqo%2FbCjtjjOFfn0ZHOxHPbSZAlT4%2BmHD2v5%2FB54klLpf5YfdRomoF6KlUrefxerDLJRBu4l9DdQssQL6GbyXRDgBmuSLS6uEFofqQeHq6ua%2Br91PIuUt9iYzamS5PiD17vpVF53b8b4hr%2B93tQU53%2BSRQ9yLMY8GjqvtbgarwMPzAkMkGOqUBlPevhsZtxuW8ifnzlvwwgkk%2BlivaJ80RrgwM7G4t%2FiTPaKAoVXBTOSJiMxAT3WwTfePYvE%2B2kLe80h1ngt30R7Qd0pqqSYClaYv0SQPxAEQDjSKs0AaaxrrQtAprfgq5fwKle1MAI%2FSqYBd7%2Bt41LBTLiDVVHbmc%2F%2FW3G8WbmS4IaCXHikUE8lQbJ9eYqmJXIKqAD3PN1JbIqyJqmT5CAr8V2jx8&X-Amz-Signature=67d0260e2ce508b42b68fdc8605ae81855460b8a6dd39749afb47ab6843f9f5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
