---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RNBP7Z6I%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIE93b6eScioPXSIOTV95Vm1uxTO0YiGUjpDFvKLTFTmIAiEAn1VpM7mAAUt7770KndaeKFsDlAIsLlVHAWauoU3MUtAq%2FwMICBAAGgw2Mzc0MjMxODM4MDUiDLNQdfydCv8KeNj32CrcA7FFGo4Ra7kNKCgxiZD6G%2BiNZ6pfqNS2ZyVpyDoMmUNz4jB%2BRCAG%2F7SQ48UqZWpG%2F2QEzPTPCRCnn%2F51DUr1wyc%2Fes%2FptMD0W0M8CMv7KrsBS34D41H7Cvw%2FVWbvWqHHg%2B7SeRKz4bmVm6zop9d1sLWXcajsJyqpe40OlaxE3hkydCw2jraRNpVYNJaPvZ0UBuCyNdJ6y5zxc2KzCh3RgAmpgwmvkQ%2BHwDCap7FneDDa1H6rj7UFc3zBM7HhKfmXpMbe8gDlF3%2F%2BsNx%2FoauHGK43ZdYIECdvFMyF6rR5wfLQO2zE0jd5dX8tiRH0gK6R46J%2FNFd0zoAdGuJbKgYbYOOaJpG%2FVdcK37Sf%2FBNrKcGEWygAkctojBNi%2FGwZ6GeiuN3rAUAnjtzVfdPtYwyEUh7Gu8QyCvRRscFevtL2YqapvsKEmajmM1Or9auCZNSK45d%2FdyT%2Baxbs04wkVLYDxE0F6Emu33ow0aj7mE4N8OIVhnXai52lzcq%2BR%2FT6gvQBJsDc6EBGtlSOxBO0Kt5W14xftBh90%2FvljuzI%2B678yE1YDOEu1mf1bDVM5a9AJx2MeVgpy4Fj4ZKw%2FRaRuwpa8%2BdsN89KYdn5oecAB7JFJWrFXtrZWyvFXoBBIzxYMK6fgMkGOqUBuLCN%2FZkZrqZWDj8r2kjw4%2F%2BZKGhDhR%2FH4QVp1CFZB6dDe60r%2FqSfH%2B%2FcbHIfpt%2BjqVWfw4NI24M%2Ft0QKjw63O0N10m6gyRIXnNZUVRGgR7E7nmulzdjNwebnVfvGju5DEAFhICo%2Fe6MQ32M4XIuOqrcsZ61EhBIvU3QAOn60WoTHEbueOly9uFRs9ErLhoIK7SWWWsIQbviY7pOMu4FZbK2WF0j6&X-Amz-Signature=4348d4d18b18a5fe5ad4a34a6a8ce5b213aac12f30108c71c32adc07f43a0470&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
