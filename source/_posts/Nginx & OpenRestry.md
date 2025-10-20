---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CTIXSVT%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T170052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJGMEQCIGu8s2ahF7kDHoNV%2Fa5H2aIY36Me%2BAIVoMTfYVrV3hh7AiAkZM5eA5RAomTQx0l9FUKuQBXr7B9Jq5fiz8SaPubxxyqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0pz7TnzeZPa%2Bh9Q7KtwDs3U9eN3QTLEpZ8Ik%2BXKvhQd%2FWFrvLIzNJ3fkwENgKT53T1RKIXBRoTAU%2FGGW6kg0DihSwBsOPsaZ%2BQ%2BLH54%2BjmBsaygEdG8XWi7cXnbaltXwi4sic5T6pPR6962bVS9jvPnygN6wosRg54PJKxgaFf9QdXDwk0tqaMap5kSOuBeFY%2Bw9p7HbqFEPomT6d3A0PjyRiPiSoJ%2B%2B2p9Tpd3yuLxKoRTc%2BsYybqMj6mdtz0PG88TRYjpUjjIFShaJaiPAZEAjT%2Fgl7%2FM3fS83HHCTS2JkjoTWcGTTJeBGdcy8O97KTfBczVCeJN2aLznUlgN0eweWTrHUfRing7FzoRYaRBrH%2B22%2FqgEG6K4juYJ%2FmMx592caKmUpmMqUVUHBd6cT1xhh3auwhBXz3Q3Lu8VSrAGLuX7xdfvwBZgsuVZ%2B3fj6rqFpM1MCpxv9cANnvs822Wl3dvvJUDPC1gjJSApmE8QTvGlecyhVGGMKobagylkY7uNVWpqBHxBGjkWVjokEhWE7neq6A35o5dQyRxUMv1fZftRCVI6rC%2FuJzSltmtST0R7YBU6dyGHWGhw4TOd1osXWFy0cXdg5qAtLbNCxYiTb8kqaLlAbil4VVaWSjekmo%2BH62WlYLeEIt0wwyLbZxwY6pgG0SXovxzjAek%2BbGmblpkSWMQfuvoLnB3qAWiNtygvjdVAYVVmDuiIi655nKAOSg6lNqTJ6o1XZmoJvGxicFsE0PqdEMp7%2BhNxaBfw4d%2BJriAB6yIZKhWZC4YTVjymOrQSDf%2FPVs1DJFe9gHMXn24OJ%2BggNsUvFVsOmsiINnW2PZGxFDYJgxlWiVtOcdbmGusoc2350SPOPvbYF%2FPqW%2BIviz3oOt11G&X-Amz-Signature=d6668374e529628f62acbd402713c32a59eef8005b457dd40e2feab4e4b8ab1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
