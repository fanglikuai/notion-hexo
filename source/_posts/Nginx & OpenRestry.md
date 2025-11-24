---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IRU6RCC%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEYfRW2IE6W5w6drL5Wm7w98%2BOH6qPrNSdEblHU08GEgIgJSKX8IuWVEbZx5ah54p5IShoN04Mu27o3Vk9kEA4XcUq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDIXK5Szpns%2FUDwrtxSrcA9TcyZEe5J6zRDa%2BzDvXFrYlxr6Uw9uKs6Tcdvr68mLJdJJT%2FJBKAE%2BO3Unk4Jpf%2Brp5nn91nfsQlU%2FGnVyZPY%2FFgJwxyb3ElyUhFrR3cl51ULuhMkD6SslGpYJH1tx79AfRIdXc%2BiMSYiJPlbpAydqKab0OP9d71DpEciNzwQw%2BFGIHABFv5iKd%2BZbtU4tlbDEADxbU35m%2FTvC6RbHgh5WGyC1g93T8J9ZKyX4bk9ftbPPSxpAddAPGNFHtOmQg0FJyqAiP8a1tVmIrJ9BoIgr6SyVu9Dm113O83Y9rJS54DD3MLBtpzeZui6mgdECi%2Fmcu9QrtsgwkD5VTejc8xtB3hFRSv5UgXHivCDT7AB7FU2G9tMMw6FVwFXupmAqyuSa6rXhOPyrg%2BqixJnQ13GZbA5%2B0z%2FQO%2FtYkn%2B9BLE%2BOHwRqsMz02Dh%2FbbfQ2ZQhHpB%2BaNjpCn0SAZ4vW3ZinV%2BzUuHrbgInei4gpai2e0LZSkRtFWPO5qEHsZ3z7EOyCduJk71Gz33gRFXPPNE2%2BXwshkKETttLFlBcgUGMVdMJykTeEsRPwADqVA8O3BrRKqcabHCd%2BhR54naeOvZ6nKjXR0x8WX25u8n6oofvtvxYIWPPa41lFsJnyWBoMMKEkMkGOqUBf4BBvHqXe3N1HBFO1Gsc%2FyZnd0wHi6UPDFWKEqBLfma9hW0EtP6ybjQPo13vyQk6uTe41gmQtwjQfxYRqdDO1MBt3QYQSAVaeLsJ3iMzaWFV5vfZW2ehKkCtJjJ%2B65Ww4uz%2FKWZO17Fnxy1kPHIjOna9g7MBC8VInlFzLtovF2F69YUMXMjF0uC1JnsTq%2BwNvgC25QMu1coh0caSq2II%2BCOlvPuD&X-Amz-Signature=5c578d5ffe626d950f29f0041ec5cdb5528ea0ced9d31d71b11d39caf5b39c44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
