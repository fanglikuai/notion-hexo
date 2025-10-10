---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNF6T3TQ%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T010045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIQCuBm9aq%2Blx1Qx7vWnR7sYaHpxBH8%2Fl5Yl0gWi0yLvG5QIgKAZMbGgG4Ko1JN5xF8byTpt5xI0PB1D96zXdAzFCJ2kqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAaSIFh%2FnWNnbqeSryrcA23yQDsmNhqHIK%2BmZo7oNfdrIpD42pW%2B9EJ%2FDXT6AoxH3tEDR3y4%2F8IO4xSBWLumJlEDap8cAhLq%2FfR6zwvyroNOYbiBZfiMhiP4cmaV041iqSzhMOhQRKq5IjospEFY7MA2HXE7dHL84WeKItekjwCF7dvPB%2Bh59fUcdlRkvd3dQyzkwop30Fwr1df6zTUj02euhL7lJ73cceHnFdLs%2BeiwKX2GpTxe8Bpy%2BtBz70C0Wa9x72ShHu5KEo3Bh1V6UVd9c4KCwmg6jG8HdSNoXZMgS%2FfYpT0vdGTlgm462rpdfxvT9oWAv6ddPSnpnz%2Bq5jP9Dk9a%2BZe%2FIStN%2BrjN3o2bizOuZVx5tofsTIn4q2x0bf3CCyt%2BhtkcOQGeMLYGIfocUMCSU20qjbbPA1r2YOs1Y6xJOfo5w%2FXvQfAv9Rr1ihuB2JxUsbAdH%2FmKng1XNQwCdVu%2BhIHJ%2F061vSYxf0wNhMKm19jMNHSuqmgZ7u%2FNIX5bA8m74OV1dt3DA%2F2prr1ptB44n8j9VWTipwbQiLeWk0Eu5uy8%2FIBg6JyjUwX0S2Gr5ffTb85%2Fctg2%2FvMPz4avEQSf8tKQB29ahDixxtLJQIA%2B5UJRdC%2B2cXB%2FAXllCd8K6Rz0Qn%2BGqhpDMOKyoccGOqUBDnI6zsKonwFAct2LjsmnkCWLggNSEZ7m05a0Vaxshh4wd6Do7arlxTnepIyidGYykhFgCpz0xJegaXKjc55lnbkgwHVM01H68Uch85fo8ldQurmvs0pYRJz8OGU%2FCHYmG1iOiOLu89DCsAd6T%2F3DFh7nPxAP7lgE5CPcWWT5cwksaFm1wpCqXWdW%2FgLCqLFmUG4p84ZCLTGHlHovedM9RDSkQ8KR&X-Amz-Signature=f29f969de604394cfab76d6f0d45cb4d5f17646eb5c4c62332f0a037f30ef550&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
