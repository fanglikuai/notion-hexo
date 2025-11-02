---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZCADVYM%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T080038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDTgI3%2FBDc6sgkz%2BEjIrTDH2pOD6SJJIUX0A1RvoXQ%2BHAIgaEAc0JpCyarsHSoYebcd2oiezbvXmvWTTgYgqOjPjBgq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDNmkEvtiHjquWOm0XyrcAzVBXqTOUuqsPkWG2URwPCSWxuWArgdn2YpIJaRJ0w4MAlrh02qtO132wu9j2zzS1pQIKOb4LYZBolCkReAZKcMb8unjzkd2MWfwYkO6XjY1bn8pAO0oeFlz36LBMpGLCLIk9P5Go2JuKyb7Aoi%2FhYCiAKyzja5b8HZjFPDPS%2FNdOykja2J400uoE1HERsF6x37Ju%2BL9jtIM2zaqEgXJKDjZmW2rVYXE03dp864oapM8kyYmTTUj72QecQLGb9opYD%2FOtFMGGhdGU%2BqUY8X4j763prWmBCECbzNpQo1xv82gpPUVrkHAE4gGRogYATJfELR%2Fk4O0vK6NBt%2FNwu4E0xHHVDpgQa7NGlrG6iFUV8rQGsf98AaTopxp3m8RQjHBvKqCWhK%2FT25VusRJ6iq87FoyYvuLEkYdJ%2Br4OnIPqGplcG4989NDl16xIw3vGGowEo8IGFf2fJEqI%2B8Olf%2FPukYuJEraQGsaHJemhKVCwrdGrrOaK%2FsejzhMQOAqXQ7rYA%2F3STvjP4rlyJ5xhETa5ULEqWhdQAWQCQLUO%2BmY1yfgpYp145PYqLOKyhSq5YbWkkU9WnzF%2BqwmkcHXnEzjj1rwX8IEULzQrdenecm6wEEGogWxJju%2BGc%2FAGk8HMM3Um8gGOqUB2EIP6cvrVFCRkaH5OPQJvaqRhx5NYwkrNKhmCGvhDCozknT4sDfYIvb6KgM5kEhacwwXfhjcKB6M8dNsJeUk2dBILmbgkIgE5w%2B39ZTfsA2JXxZ8gyTWjEvai%2FrunrbXGmLn6iiPPxVAE2dkNkt%2BAevJDVDgfTWZLir4rNYhww4s3SB%2FzG7cCeL9XcRzRZz1BF96rTap0hC6sGs%2BJMDIkhcl1qSs&X-Amz-Signature=f4fc2a0483f419bcedb5474bc75743f13e8c426710282bb7d3c6ff1380c2b495&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
