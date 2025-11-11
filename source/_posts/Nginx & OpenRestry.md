---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652XGF66L%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIQDnOBBRp2lX2Pcy118ZJsMJtHXrojDbQca19dxJ9%2BEmzQIga0UQtzFVzeMw2IAXkZHwIUgrCx1M6djsy77R69pEE1kq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDAQg%2F%2Be6z6gH3kF3HyrcAzgw88GkGS9RmlPRMaWg6MRZ%2FLAEJyQQoLkJwwnDJaos%2FjC%2BUieVFtuPGKb2Wug7TYsiVlcq581Eb6OyGhQnOxR1PZqdOd%2FkLGqwgz13m442Z%2BIHWaYJ6u6B59RKQuZoNTX3NYHOvZcbhcSCegaWoe1U05L9O8FBRISuw1O3gfcZinIFaydJ544%2BUmUXXZ1Czp1WYhKBx1SNfmvnkUWDdisc%2B6ujrXPSMGLDJwkwB75JC6mhyvkqf7Y66FVKTePQ%2FTa7Ed3WAWq6zFOsH47ILoLewhBAWwviXrBaH3%2BY%2BFSFJzGAj0Jmm1NTFNV5s7nObq3aPtpxJtXcGQuWM1e3aJGNccpFB%2FSDUh%2FR%2FPeEaaMhWI%2BkVhU1schZkzib5OZAwhpMpGzD9p3TuoXp%2Fe2Io5kTcSvVRR9U7K2nn9%2BCkLEEBgWSdvX09WzqqM%2FvJa%2F%2FF3e3kl%2Bv%2Fhaeuy3wXa%2BJ9kfGOKiNOYNfesHl0%2B%2F3oN%2FONYA3aQSxW2RIYTZLnkf5V3k8Z6hwlx4XJb%2FtC1Yd4w9JVo7mMqxt2vGep20SrdjhEECH7rd9uGemURggikA%2FL28TXv5zdOJMwFTyP4cg9bWzDecIi7NnB5XXswp1KuZRsSGTOqosuf2qhjOeMPLnycgGOqUBzqj2o%2FDqzZEBUH6HIZDwLE7YZti6huYLzxVRapXGfKYE01wtfOgSTguPqklVsAhsv12vPJblVE5sm%2BRza8fFM29FSHQzEgCNwEQnmpSFiqCoXVEZmkpnkQ7wSMxO9vUWB9L4h4peaNXX2VH8mT5eZffFhHn2irwBiJiw9nen%2FnhlZdGSvBIV16MWFept0u1UdWoT3VFci0eS7iekd6Mp9XU40z3C&X-Amz-Signature=776c83b9ffc40a00425e99333c24b9e485ce634a05f95794e8edd0a235d86641&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
