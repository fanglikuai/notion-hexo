---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SS2PELF%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFpCR17vbz0vdMdIL3At9kynbkpwrpVAys%2FEAyMPvMVyAiEA62LRU6B%2FZt0y1GHKEqpw7isnvW333ZLGikbmBxyt2CEq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDFQl%2BtfJIiE%2BA8A0eCrcAzobhb174tmYWNXLCppp5wihY0AvzN0NzvQeg%2BXQyVHhjjUjEE8sWpffGBaVRpRs%2FyP60WUQM0ynKIE%2BRH9c7Y4h4YeEkX3%2BMnRFh7prERX8f72vrcLYOyPZi5y%2BbccqKihiD46rVqZ%2F8jtxte4%2BrMuC%2FU8hPwP%2FJM3MIEHtaa6BMbKCIN5yw3QZ9d4RrJ8NiXb9NWPEBjYZ3BxMrlRQ8QIn9T2r9IfB4SCaX9wjfYufTMwJxYMGTLyOvjE1TZ4Q5jjqm6Tfj7l%2FTi3iO%2F4WVEHNaLYsy832XhvwjUVvIcz98JJgwSq71XbmUMBiVFO0DXos1uTmQb%2FsUpPGKGUP8kM5GHoUsE2M%2Bk58SqPnm3%2BBPgKyoezG6dmMVqYyv7FQMofM9JkGwfEbGYITdGpRq%2FVDd5ENyhr4Dyuyjkvy0gBMdAuxUIuBWCmTn%2BZl3S9uEjSP3ligvyg7FJMDvYFUiLVc%2FuhQX7yKvbkcaVTFh27e66P5mJVZPafVqSL0%2Bi%2BntyeUvDUu%2BvTAlwVEb0KJy4TeZAvmnQVSmXbhFsZdF4%2FNTPdLN4abz4CWhvNKy38Ph6ayqQpdKszyXQT3LBz52BZ07XrgXIAu3c%2BeJIYGVYSm8I%2FZse7JgcTxgN7mMOiyyMYGOqUBtuviNzB6Js5Ijbrpt5FD%2BwyzqvEDUxS3HoKS66vH%2Bg3z2iJsmeuJS17ud90JalX8lCrYnRoyMSCtreVE1LkFw5oymI8rNS9B9hSf%2BTI4NBd%2Fqknha%2FkTtyiaeZWbTlJGZLQmA8OcSDCcwxNEOT%2F9BZShNoHntfoBw5LHP%2BPaz9DE9VHRV%2B%2BFLFsDRpuad5yZmIiSTHNe8PYG1%2FS%2B72rGmNNLGVPn&X-Amz-Signature=c328751fdcfb52ba768d152c3117dc34039cf20ee4f053ff4ad2c81dc0440da4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
