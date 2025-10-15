---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EIR6OLS%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T150039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSkIalFRpLYplhnrbI3Jesvoj42Bry1pyu56gIGChyWQIgNJVT9bl%2BrsJfNgNz2PeVadp0onlHvvGA5evWiqQ3w8kq%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDBts4vz24VCqCuvW7yrcA8rR4VfalpSy7lcsa%2FkTKJd4ilFzmkgXdfjoYeboPrQTDR%2FvqRAAT314dwQcBOqNpgv86%2F1xJK00Xk8LlTv7E2%2F7%2B9AFsdi%2BuK44ZIZrDWdFS2bYYveid5CLO5IJ1liGTZH6nq0lm%2Ba%2FB7JIqubyngUeTTr2ZjPMv1zs7btYeQVvkRCVr556ps1VTWZ6bUSSaVTXe9NsBpfWHO2DpUZhhlRd%2Bj%2BWly2jGl%2BOfdywtXR2FMwH3jipobZGPrZBFCcUMpOOLh9klpYXHFW2u%2BvTuL99wlpbSEB4fazC1HS3pdoxqdNfXwDHEOc1Z5Alf5pKNOFk26kgVLkLAFn8P2nfkkNpR%2B6biH7UOpO5W5nEh%2BH0QXnwRI9zRmzt%2FvzTOAUf4ffRZfUZgsJ1zWyAxTrjvOHCtJyKE53VF3aT4aBDJ1a3FNXdGR0t3hh14lVsSbEjqvn6TX57XoONTxeb5aPUNa4aLgMT%2Be%2BZQbJaxcwQrVOlQeMQ9CIbJVzbMm3i%2Bj9Iz02f1vBLnWhp0q5v4KhO2%2FZO3qCwniRpmQuIdUSDtMjCs%2FonZuC5NFsqe3kIDlk1fvBlasuEIUoM4XYrgYYKQZqzV20VB1zNl1cpOC5JbIM6Gh43el5IUn5feai%2BMM%2FfvscGOqUBdDZW5Z%2Ff63J3YdjBlJ9vD97BYJZ1Dg9mtCO%2FhrcjsSuzvatKPSy45gp86yTAuCmjrcth0RCnkx5PCSd%2BqVawP%2FXlWkU%2BBnVGbC7KVvGLrE72%2BaLOt9%2FsbJbEuKIYWs0HFwwQZLgjCv4e1gDPmcmJ2DHUxtGqu70Z9odaxi8hC0IF93sKAvbI6iClj5%2FgvrIeFCrTx3KbCQzp1bviROFnNN5dLKOO&X-Amz-Signature=c6a0640e69b5fec2b373df485cb307a9b93958dc18f74debc4f6d2d1b92d0709&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
