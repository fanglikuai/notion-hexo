---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3ZYXYEZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA5RioBok13lnpoiWqRcVxVM0ZmZ7%2BfCQuVF2cvukdq5AiBdhmJPKwLmuaKmGQfZ67yRRjyUY597lzZD17fuM5gz%2Fir%2FAwhjEAAaDDYzNzQyMzE4MzgwNSIMBFpe4mrfDUojPwQGKtwDO0UJtfvEagtvZFJBIDtSvy5jH3qex%2FVX5snpmEkxAlwCbCrI6hQpXfX5%2BZ4owtDcFnMLQlXLp4VcHCs%2BfzmDrVZIyq0T%2BTUe3eUX4cgqEJdFLpVprV2QyhPrjoF7S8mfLp%2F%2Bzre2NPx7AoiKfLLOTf%2BpPw58l9W9MmGPB4dxzZDMqWYWxlQ6bU0q1f%2B3xxw7HRLaXT2ghllCaBuP7zcbhxv6JO5USpZgU%2Fw7KJ3Um7aDLyKWuZjsn%2FlAUbvLohIm5Yen2AZC8nsV%2FclMDrIM2PMMXgGA7ssqT5nGsKtg4LTroe4HXcfZglRIFPlVqZ2EQEcWDqKvwbnOz73D9%2FGgiYar21StazGVZzH8%2BlrVAqtS2FOXC0T2zZBw0kqJOQX8mXJXl8mFUjtdP0Tcmd1dNRs2ochgMx9CI1Z%2FRqbQrcgiDT25OLP2RFkmhKDM18av0UbpGNehtzaoumoiZmbuVenPus%2F50wJ1FWkdC%2B1YxQr5wsPLm4zfvX4BJ%2BPq7vIdHfAaj7vy44IdeliG6JI0u335iEwWqEuFCcZjV29AcGRweXZUzLIAOPcoLIIBJBE73vRNlT%2Fxzs02GmA58I%2BeU5Q3D%2F4R9HnBN1kNH9%2BN25m6lGZsGi8b6xNNPgYws5i6xwY6pgF%2Fv7Abx8Ps7a4bXTdNfA0RT%2B0Ud0zHOwqDzfxCc%2FqKnvrISn2DdwWTLZbDC0bxese4bCjfKkF7hN0bWwiaUAoe5JEYvJVXqw3GEUyXbpFtcx%2FYnSqAxJ7oYqprUMCWhulSVe95qZvASFe7LNRPCusUuWpkAUWKEQSHorXGBAbuaHpa0jD0M1YIbVjCIexh2GlOHKEuOQs%2F6SGqj7zmXH3MJji1QGQ6&X-Amz-Signature=62940ea1a53902cc8945bb1366d5edca112d7ff5842c0349f07e2b8a67876a4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
