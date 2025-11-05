---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCIPOSRH%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCyEv6aAVkkZ1WH3WgJS%2BJuSj9JMATFoOsOSrevmdjjagIgekUQhpvuc3ty7oAhTaIYkXIadvvhiS6QiTMCGIHS%2FTIqiAQIjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKgrkua09hHgwnWlgSrcA586USsXUfEvmvRKfUoma1E6pT0GT3I1eaB%2Fetbp7MHkmpkGCEKGyQq6FJRRlzE0Cq0Z0XRBaqsfZtN5gyjxVlxjWRslB5mNASAURQhQKeGbBhidbMz1hpE34H9zckNZ5CbgnCfuXwVpnl0SIwIuv4E%2FUs2kmYDQW%2BOykztTaUBxbdJ%2BoNXu2ZtngqjXNr3zPWKsEqo98hK8Ky7k7Ih%2FvbhWLj44%2BcpROCUd%2Fs08vwRBFnioHT5tTKYHWz%2BIhxoBDVxpHjRWlh3e%2BJ1vAc0tWUvZGvrD5Ie%2F96CDL6p%2B%2FthTNFDAV5FHbfsiJT%2B2DXPcP366V20DfzTWZHra3t2xQjmS773iqrFgUeQNi1pdQiAcQ2P1Clc%2BthMk%2BQ2V2Jvn3TBVTHla2D7ws3%2FR0Jjl5UVIBWrM92QXsodJbBHKmrCCrxLJULwWDdV5jQL%2BAhWhULIswJLrLZClkCO27U4Z5zL5cfgZmCtwkEdT6fClQlz1mZb1pmpeJw0IK0dt0TAmJ4EFE4fBPgOE3xOUlRFN3kjBc2r3OT6601Qf%2FPpID5W5K0ctxiYZ45AAqgQtnJwFx0s%2Fg7G18Hv8V%2FtsntBkcQUS6p4VbyxJjAsZMJckWkcIZ0IVboFV6FXnPmdDMOmTrcgGOqUB0tDWKdBANPNYUrMuzM%2BZ6JXmkGE965%2FpAKWh%2Fn%2Btir%2B1C%2F%2Bl9H%2FHMtcnfW5SNgl6SKsUdtokXng3XyEWqGBCV1%2F%2Flz2DjfzsNBTIQuaaq%2FAG4%2B%2BEmtzl8rZJZBF5PdPYGr8ZdekXd9g2UdkkO9DK3dAY74zwKXdRDFOQR69G0Mnp%2F2KHx%2FZyJ3cwxALM81mOw02GDBaNUFmssXzsMfRYyTGi%2BBee&X-Amz-Signature=a32e9b546e28cc9456b80412999a453e96d8fea138a3950d86f542a4e31f0388&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
