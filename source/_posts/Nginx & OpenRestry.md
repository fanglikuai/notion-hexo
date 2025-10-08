---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46623GEFVMM%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T090052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIAnTzovZI4oxQ0rmDFN8REjClj%2BGnUf3dUJE0E2CeEpoAiBzurCAFHDrKL62SwaPclPqroP%2Fv1Q%2Fgd1FFSer%2F0ByHiqIBAi6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMRPzoKjO%2FFqYeEda7KtwDmaLdt%2Fhx9nPhakb6H9DRWf1ve1%2FsCD%2F0A6MdKh0Amp8N4d07hiExa9M0G25Dikh3DRFcNQVQU4SIfuIiGCBOPPygzA8JEcYquIUVeshpMWIehNYm%2F3BYGxcDxoncAt7i%2BLsejq%2FHu%2BA5lDRj7EhHDstd0u%2FmIrx99M1OzUYCInNAiikIU6FK2gXiS%2BwBbRX7N%2FIYrAkVIsQLR4Ozlarwakv05aKtniS696LPxl78ct8P6j0fi1L%2B4%2F%2BhE9j5hOi0HeAl%2BjxlKo9gcj8yRyD9DUB0aGL8Notm3cwf5QdeY%2FQnFff9oKwhRxGyAFI0sFM%2BvOzKud7Uyy06Y4XJQNvxna8HYG04gfO7xmFBvMsoInvsTiydpZPPrbY14WhA5DpX%2F7dKQA3EDm5%2FJpEhqnh2s2eUg5YuOoki%2BbZOklhR2qsUJ3zwzUZoOoAy6qyXIbzp%2BXD%2F4eDYtY2ZfVVQSt5A6V7j0jVJdMiGDPUCMjKNrro6bWA9RZktsyySpCQXlTitxZLOo%2BOieXPQXGyBUa3PUvdpJay32%2Bng7YGm6ro2yPyfftdIc65eSK329jsEA2LQKahIiWUPVp2cCm3SeraaDg4ZLwF74Y5iDbq22t3%2FmUDnRNrD4t4tBWBe3V4w%2B8GYxwY6pgFHIISz1mazwkjTNZcgQfVjRIbIiKlx1dP4sdKi4csKdx7Nnwn%2FSGVFeIaK7gOn9NfHWhMb%2FJ9PvzbwC5uuL00js9OsPFaZZ%2Flr0qsm0nh6D%2FcYiaYh%2F6wDSpXRulausrVkjZ%2F6z2IkoMIaboxKK3U3cGxtiR%2FK%2Bv06Qnhm5BU8v9eH5pHbAv404ij3ihwZdd%2B1jp6zdzGO8JZTBfj8bwkAc2PocY1M&X-Amz-Signature=4f727e8f225398e3bbd7bc961891c8b5cbc3f280b0beaa947afc96c051095819&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
