---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667A7UF2AN%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T050039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGCpRW79XeYReTLYK3qjdVkrnyA%2BN6wo0wllQNUf31cKAiEA9Cu%2BFegQYWjeNza928TLcNQBhK1sJE7svBmA8cTyUvkq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDHgKi9%2FZoxAmSuKDKCrcA0Dg06ngo%2FgBPv%2BktKmzCBCUA0MvT%2FRFfhE79JCatjKRt5RxOJJ%2BMsfa7sET3JjAe2Fykc3wIzW7bHlpjyKBIw9mgZjUUZ9r7zcfiN5%2BCyOdEFMzw3TiTBljRC2YFQxf2AI34fPei4bWy2wHI%2BZ%2F03A9ZS3Ud4kDxmtN9i9PIJ6IGCIRvgsYvZ73Syf2iILOdp9vE7rxeZOWNUB8QfLasQzUOjMWT3jR96G1EPXdgXIyeA8UGk2AMLDNhfZqB4o06XUARrRnaheS0D37K6XugWxzkRw%2FFbIETiAvtaA3RnTWkkjsqapKpkkiGizniuGDCvYeeRZJBC%2B%2FAfOtwGRYnJGL1Pljv91NzUXc4HVFtVNdZxadmfMDDIdnzJKz4xbtNj8cI8nx0iw6%2FhQTfxQfGmEI0xSBxHUC4%2BXYBrC0G%2FmLn5fMzpEWrR592vY7Xfq5s%2FK5C8q9D48jMUC812%2FR5VMp8lNMA%2F1kS2kd9PbomuGlOoMjlzmKzAc9LcF%2F4cEYit2m9l04YjKVb8a3FRsqVbV4vve1BsGC42sTrpLpXGpjxhTiIkZ6VBsOPsJTYAeYiyemE8xII%2Fg9V7%2BmCS66MCidYiFV0bEA4bw64RtLZl2dG2xmczHUOXBLEhk2MJvQyMYGOqUBctcyyzvnNJf79C1gaLShOM1Ls2WlrfXdpuuceKIpScxtRFZ1iW45Wj2jlE69uieFemKhQqtAEspDFY5cCo%2F76rtnRSyCR60vqc%2F1KMy9GQ1bCJX8p%2Fqlgi1rs%2F3HCQO6QRhOUAU%2FpfMtSF9WlzSQeOTeNIcbtUSXp502ugnYgWKhW5V8GV52o5Sa3B%2BBy35nfh3GfOs2d%2F%2FNWO3xzwi%2FPMHjy6mG&X-Amz-Signature=5a5d2e82427892baac9d11bc68eb7535ac21da80c9c2be93e4f2e5227beaa998&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
