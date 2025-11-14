---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PAFPKIZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFXh55A4nlPYVtmZW14iqNcIqE10Vcoqiox%2F0Vy6HanaAiEA7zxFGd6FOK2Q6bTsnq%2FpWE3lBs0vN04wO0C0C9pM7eYq%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDMrE5NvOUKYgWuSDoSrcAx%2BfuNTk13buDrCbgiQC8jTiqSv1LK6DowCDivSqbPXqQyYoP8LV2QB5rclOPU2581qqkWi4b%2BNTcU8qpD%2BFHLdJR8UOCzW8bVzoQ0ag1RfwrEi7ol%2F1c0EhgsMWWLUndiUbOthbDwlVd5DUGL1GLs%2B9lP7Y0YhDpEakam1oq%2FdxsJRaMYLqQppXVXdqO6ibNQE23OvAQOlUCtlUeVFHfl6dhq4Vj1VDLX57%2BAZPJGzjUYkD3w6SJqFYWWti5W1ngzdf2Lc0qnOETFUtkbaOLzS0HIWBtRQwNF8r0DY4O1QccGTsksv8BF%2B5urpniDLxAxHnuelMyRNJ37qorsnhSjwwIANpZxfkO6U9MZOHF%2FEdGkaFr9YC0axDOfmHEgyuG3rDjAF2RHOtS5Vz%2FdxMSnpilE9HFfRcczn75ePIJETj3USI4MtKXVCEb%2FkHkP1HHcVDdSMB3l8ebiYvwXlSEhJXwUYY8lxBMO3jUNWCiXNf%2BLswTp2inAQapr09zyeTsYkKcNBqHy1KMz6l%2BMwnHSvqylJffQjV9LSkWZmUsGwY3eNkO6kWb4TQjt2pDlNz4cAIUS7lUKLOhNxyBU1ifwywU5cZJNPmr404bWZ%2FpahMp1AeetnUTm5MP5HLML7d3MgGOqUBsCvBmMogOCYinJWApmwsnZRFxBFEY%2Bes793CkmmAryXXE6eQ%2FOyeKpOoA9QcCycYd9AgXa%2FvOpQnN4LUlS1Adjmvc5KxNVN0ZRwXlHqtI4QQCXpUD1YhZWa1Xw4StRUVGlXq32BURKMwFaad3rvlEiK1ySHc1kyvddUyTZy8aRdPj65hOz610eeM7thrX8Du7%2BNUSGvcXNSADdiktYbUi5ZWyouU&X-Amz-Signature=417a6087c31df227178b58f496863b7ca969d4e3c590272a152558e14690f1b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
