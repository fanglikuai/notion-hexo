---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VOMX4J4R%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T230047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbRvNpjt46zA7sZo56a47CQkA2qi2vOXdiaO38hr%2BXIgIgZtiSLhB8p99oqNqj%2Bnx%2Fz5oUAG54Cw%2BHvRij00RdT4Qq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDJqLEBt9lgy8LnzIBSrcA%2FmcjTNIyyKlQhrFOSvCKqlc0%2F%2FkjJQUQ%2FPHUl9%2F3RKcI58YS5NzzFJ%2BOMuCaaR35r7NLJjT3ZDi949HTCy%2FuuBpDf%2BnvguxyipR7B8ycAChcO2JsKRg8A0fTorXgwx%2B%2BWgKYSzbcAvGmZEtacUrmJd7%2BKExS3VerPDu0ZYZraWcNNmjCO5%2BUz4cclVPcf0GE2SUpc92EZsWcYi2g0zX%2FD3W8JJVBvegFiMQEJHa0lMOAVE6XadPMJSW6IXX10QAVj8UtIpDtqpUKeuKR3sD2ryhj5MrLSvrTcIHc%2FwK2L%2BwCxBatmFf16gUFQA5bWmUsRZZ7448s3UQeoJp7fM3BpfwcwBzIARXum3hbRd8OsBmn7%2B4UhzHuVN7DMM%2BdV0MJ3GbY1ATz6P7%2Fg8oGd90yexal%2BHf5OAKsEFQ91y1laMA9Tb5DYeeS3WrcWRvKMdVEwAOPI6jx%2BGJDqYaAtskwRh2bB0qAN4pJunlBmPebI1wcE2nqpvjbdPZkJE392DX2grkfB8al3GmJB42LP8TA9CTAEh4fSc0vKvqRBl4EWD8JQG02ru3tLZ1eeLCbAMCNpX7Z9q%2BIYOer%2BskoEvqyoHztiJyYyA6muQkdPHFPQcQgdY2ezAzBfLHGlQyMNm5zMYGOqUBSglh7JV1%2BRlwIIgf3hABloVrBJGsDY8nWNDXfZgXdATFktzL7bFMQn9Ch4bNOiub%2BbTNiV%2Bagf1GfW1zTWrx8ziFz0Pa8BjdxmLO%2Fo0awrfNQzyPQXdvdEyI0n9DxXAZAfhGLJd8HS1Gtu01%2BlmXHTCDCzjqMoa4pH4UdxKZkJXjjytlwUfr2N341cA5Svv5vwp9N61jS3sFgYSIBViQmt5eEDKo&X-Amz-Signature=e77a45b172cb66458be9fda0d4e3cf07e4a9f7b0851f54899437a81e219f8013&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
