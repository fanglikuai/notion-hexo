---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXESOIZM%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T180230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbrQSYlZE6%2F7rE%2Bj08JG4WRnd6Sv22NxxxcUtAknx5DAIhAIKDD7EQhtoKcCGDQWI6W6clO0%2F%2BVQc1ga4SS02hdOFFKv8DCFsQABoMNjM3NDIzMTgzODA1Igz3luFQ9LdudEi8pJIq3AMjSp7v0tk4%2Fe0onE9NpHSXPnpkWbIHLZs44g6Mu2M%2BCfq6SF%2B3LRT625JGdVRATTC6Mo%2B5%2FNUtBMA%2FScGIAHbjne7tuAHWYvQ9luuyECEpDMlNFIkQBocyqgswENSw0tkFOKwWpspnTu186%2FXlla8KSZbyt9z4QVbYZI8QVVq6ZtFZmytZPLBdpfx25VGnMgJtR4LlSiLQfC89tad1iVPdfsU7PTLCoa0yq3Rl1ueer7KyIp2fIyBuTDlIhFt3RGAhMNC8un1W9slQUnmPrYzvwyoPyqhmn51o8UCBknRjHaow5UQqbzN6tm59Prd6rWRnH1SJCavH1FzqHkMO2czpUCmf7I%2BTUlffO8bD2y2ZZbU65MaNx5yQU7ev2cuufIBVbBWCB9rBST%2BqA9ZZJSuJbVk13SJ%2F7EuUuurIrpQ%2BKBDqPg5PUg413lqi6XMyur93yyx87v65IdxY2PUlP%2BpJwIgF6bNprTn%2BC6rY19gwS%2BHK%2BRc5v60r8RGCa9S3Ut058dJzH1ctrgPmfN4boslenaTXeuLmBw%2BzKY4Bqj4O4VluHf%2BaGReQiCfu7YKPUewxpv2ts2fKEnjtA8PkzqFINEnHYGZ9amIsRdYNj%2BDa4vbSR87aRh%2Bu8yj%2BHjCkupLJBjqkAf1zT7JuIYclEINugl14Kx%2FffjPf%2BojbmIH9V0o41BsoDzIkUJRrbdUmraSQD7geQpxY4VDT649AANrZZq7xjIiYi0xN6cZdRhfEoFzc%2FR34skChWycukljlwNTO2ltzxwfBENeniKFf%2FF8tPhNPR2N22CEcNM02ecyBeJ%2B7XnC%2BPwBi7tQ9hFUaXDCd9OyD42AoUTYY4ZgHUq%2FKe0kJM6fxhxzc&X-Amz-Signature=e2c94273b366c3d0d723d3416eaa2a270c339042f06963975e906c57c7d6e113&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
