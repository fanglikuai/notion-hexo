---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI4W3J7B%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T060039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClGm6Md5ydTwUVENuzCPmtsG6jmY6ZQtyPanX2A3SVngIhANrXuQBLS0EZh5Polh9%2FfURw4vi1wpYQFTMGWpPT7MFPKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyTtY3e0LE1ydwvpKsq3ANv%2FlUFYRgk5RHcuP%2F0aUzXtijUo6elvgEnNugllBaTW%2FgF20bPrPA30xON3fR76iLYoXDeG6LMwJGSVLrDlBmuIkmmPHOeKWmjjja5HCcyPTrTfGCWkdpSSywsyrLxztWDgzpo%2BmvJZxb28SHTGp8tEP8CgkJtscjSlndyek%2FYKeAmxHT6rop%2Fues4G6NaAct13ubmrb9isKMVUPA560an5QJ8%2BH%2BWJUsptVAo9D5CbX24EoGeq2smY2jCkmhdYkTrsaaAm3kvx1A5RI%2FAS1dj26GrEWJX1J26mmEpcR2AqvjSyvlUdhubzWnveTnyGfyuOOZ8FPXu%2Bq0soDk8KXE0yYmrWCm3StNeBhGdlUgOHL%2FKIc1NLrKe4t9kFXrd1UXrqX%2FO3I6zpiXDjtj5Brfoo3%2FKtaZpAaisXtQ%2B53SyjoumxBWHqEEF%2BdiasciVHnAmjayS0mJoNnQ53O0aTDehKb65FPDnM%2FYm1jXEHJD2UOv9jPR5j2zB%2BLv9TLF4gl%2B7%2Byp95q%2FO0hDP50iwBfr9l07XJgJ7pql%2FxwEKVaX50BGIdwvNWznrkyTFC8LTFykcZDSU7m4Hk%2BmKGzvwWe5pJiOW%2FKgpFWtZUsjwSr5iLOg2ERyRd0bdArsvGzC5urDIBjqkAfuoMGN%2BH4tpVp5%2BgMoBYEmFpIuWnj1Zqf8fZzxJ5u%2Ba1w599jS9ozRrvLfquBfycBtCCks1iTMosmZ2Nhvn8mQv4PwsoIwnNwn%2BfBbUpSB26AFZKktlhbDPqNyRlb4vqlMFvLAfbkapWtxLp0%2Fd4mB8WZ4mj0HGG%2F7Ilg5D4AR9pJy0Yg6kANXPnyzr0ZvZ9P60NLdKco%2BeSOjKHZFiB4hzHoCx&X-Amz-Signature=a801a3f604d1f843d4b96676d5ffca20a217470d5f80045dab48a0227582c1be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
