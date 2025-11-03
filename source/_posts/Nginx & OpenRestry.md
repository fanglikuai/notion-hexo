---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GSD7VPU%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCOw%2FYK4akTUfv%2BwsBX4VIASYXY%2Bc3yJgKts6xp7kLFcAIhAO5ypZLWvuxqXtND0bmDoAKRJG9enLSnlz9zE3MZnTaoKv8DCFIQABoMNjM3NDIzMTgzODA1Igy11nQH0UUv%2Bs%2FJMIAq3APs3BU4x%2FoJSbCJK2xA5Sa%2Fd2xWZCkOFPgXsb36XjfBQ7bIrfzsacZpaW4B7xOuOIncEQ7mNDFEI4l96ZVjW%2FG%2F5zNp3YZSRi51VxxuiJ2slt%2FG97kJAo%2FapboLKayEiYzjufK8Q10DYzrny2LSAZ%2Bz7IRC7DgDtun3DsalHadQTcEg2gqVnnMzNZ7drkTKI5THpMuAz058Nr8bAZLUD1mvWZ7ElkD7AQitP%2FB62kl3qw0uwZevTHvelp%2B%2FUIAhqyRQyVdpg%2FS0QAthxn8Qcew4pIbECZioBJLISBoPx9HxsgfW5vfkwjdxrLGULcMCVEEpB3rcvGczm5scjlqM1CQQnJ%2BC7gDQW7bFkJ%2FzLyHNjlmbf9RC1t1TugYaCoDPiF1vrWMEez5gz64ZfBUBSaf%2Br9uuzft0nfOBhIbk33Rp40cHsiHphMldSKBAtzvWEwiN2GEOrwbPrZ5N6xm3c7ghJ8%2FbHWfbmyIS2nemY8aN6W9uGdDjR0WXclcGcUUO42u1sQkCf%2BsJt2qM2khSHqoxM8%2Br%2Fu6H0nOv%2F%2F6PryyQxGLyX1%2BDNwnKj0kih9%2FHvnq33mnT68aU3Hj%2FYjiYENb3NOVQx%2FbelD2kw3SokB%2BJU3an2p1i2n0vmpZ91TDL85%2FIBjqkAV5jQvXQ22vSoEsNYrecZf%2FBdB8CTsW0OhC934CZtUe9hrpehZiWiaE4ECOYHIZzn%2B0qb2xV8XD7w2S8n4aVsCx4nSLQA1F2wK89LiS9a%2BLdegvGRSDvV%2BBqD%2FSmy6lWVFrWnqgdOQN54mNuo0V9dOzP3pl0SwRIvk79ctYV4%2FcDFthE07DB6ag3vQ%2B65IDKVQGm4pekiWMKIeJNKPic44wBbN8z&X-Amz-Signature=ef137ae7b78cbac189ec3877ce9a68ee241eae9dafae8482bcce8212d4649c51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
