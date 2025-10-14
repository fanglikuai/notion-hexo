---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AMP72RT%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCShs5XHSPE6PP8XqbF4HAX5C7HpmcsPN1axFxIDFFYzQIgDPSnLoIe7pkpokzU5EkZsuAOJUnc%2BrnIElqUu1wYNFgq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDCWhRRb8T%2BSCjSUosCrcA0KXOvSGUMga1G3lbArB1YeSW9luJ4PX1uzS8HUfVcX3fLEkqk%2BkYIeQ2CyT8AxetIYVF6ovPBOQ6aH1NIjgKrsGOeY4TAdLL5gPjD%2FAm4gG9Zx%2Bw2PJDiVHcCXbQ1xwo5zRjm4wTkTOc2ATo0O%2FwECJ4HK4p%2FLAyGLUE52bXBuoiLeT4eIxV8Ab8F6dUCkHjPo%2BdepsfGtePKIXeU9A%2BEoID%2BfRXUujh%2Fujh1E%2BYJVccwdEG%2BClcLZEFDkpqm6wuxoz0NASVB3yg3wZFbfuTUuQfFE%2B71SKSYB%2B4Ng2E7bobNkV4HRXeXZiDyF5jr%2FAtEeTjb%2BOMyO4%2FYWsTvfGnJ5xYs4WIkWRpYs%2F99a%2FdVdimwBROwdvkm7JawbCdaNdKm2nVBUzDP7Fdbe4QosMoVW4IJbFH9EpjeWYq6THH6EsbarkCFhmOVQvuzkQUqXPXLhkO66nPYBsHbg2xqtxVcAZM98rwQrrt%2FE%2FQuKFWnXB%2BVMjee3s41FbUf%2BwQqHsxNThwrsP39Pfj7jyCOwpGHgFrdMRLFdUDm2eEeUHAKpkOTeQI92zPz727JpsRisay71Ja9%2FNBKgJH2jWyqUcowv7h9I%2FFqg1HzW%2FLEHa%2BTasrWSARBSRF0EhTs94MKm2tscGOqUBlD4QF1wlwRrDgnfKKT1wEC17%2B1v%2BtXJ5omo28nUAnbyMpEMFH3WiyNAjxlDytA34WoIKsqynXV6Koj3IRbvzY6HJwtRs2VCf9NDEs8onE5A%2B%2FoyM4qEVQZeTG5yqJef2SZ8%2FQoNPv0jTTt0YbZY1g37NqG5kk7ko1O7%2FadKLkp3%2B5OsDvsFvkjKZ%2BDZrczl3GfegAhnOJ6qb0kumDvH09X0fK9XR&X-Amz-Signature=7503c5531a2413b45008849183b548c51fda552ffa3848f5cb685cb0adae4471&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
