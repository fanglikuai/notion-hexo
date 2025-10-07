---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TVUXJLU%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQCZxPwr%2BCCGYkwvnm8XvcO8scCCdrCBosRvPbIU8Vc0qgIhAL2GpKsqYVZs9r3gMli5NnEhQaNvBneESFo0rFdtcDdkKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyz3CrZZxIEQzScU5Yq3AOmKyzsD%2FePBY%2FU1bBwfeLAF7EP4NSm2fcxxKWri1MaW9oXzavJu%2BfJS7nbZfz7A%2BneLW4828VDxCt%2FECL4mrG%2BInZ3QldE1I0CxkWnYutADJnEria9y4lGIdRUVm9o3cVvZTyutFCzyBWz0MrByWP%2F%2B0YISSVAahVP%2BZR6pXBTIbTei0Tc0LTKkktcL%2F8JtyA3y0DwX46jytyOmtwSAB8P0iMm7CTk8tmQSnzdfMkPYKoDU4XfyIvQhjQZX%2FYOBj5dHH9cif0C5lftWxJsFB0OMjKx%2BtLLD6L8bioMFrGsMUjvrzbKNijEsNxR0HU%2FZLW9iaHZOALoCuKwOQkXZTRJOFp61wxY0zbsDkEffn5uDmXr%2BaeNGFNsqN7U1U64bHVa6zbZ%2BxgaJJe8%2Ff9un19yhWOWuYJIEr%2BDI3FK%2F%2FjldbXit2i27K1947aU%2BbkIPoSeBNW%2B8d%2B%2BHiu5fTmorldf80R2REBjCxO8%2FdJIxGYArw1Ta6md3%2FCNXxoU8SHJQ%2FUFXeYmPkptqyT%2BV0ua2YUYe%2F0%2FoL0Qm2XM%2FnGrCcznouNRNg2QfN1bn5ENLfcukaWr7XedexH4MSCycgF7Y%2BDt7dhotIiDABeMY04EQPlpCpsbdLa8Fhqg8UMAmjCjk5LHBjqkAbfTTm7CxizWSDzGvslo1NUyICHqoz7naNklJG2paOLBLhMiwApZjlbgegFvIcJ%2FVWbHFJX0cv4E443kOF7Jdgxt9wTTedS5RPei4nGcJZcwRClQ8PWh7ZpVvh9IetzW%2BUkq7IT%2FTVNjOM6FI%2F9%2B2r5gQm%2BwZzQXhbwO9od11HtbcHbcNemDYsz1mM9Kd9pTxRHJVWKxeH%2BZRdWmKhTudrQPeqLn&X-Amz-Signature=fc5a1e6f3ba5a24b0cf984b30c552cf740cd1e414ef62c31080d120f6e8c6f00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
