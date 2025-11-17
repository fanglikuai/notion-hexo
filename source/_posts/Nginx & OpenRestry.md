---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YCJCHDS4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDWkmXgyrBwtpOwsBbRUED86wpdKDiUbZcy5IUgujWoRAiEA%2FfCUnzXb43smiiBpuTsNu8fZm906xbPbQFQ7BLrEni4qiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIjVmYGG9oNhlzsmXSrcAwTPKsmPE6DWye0B9o9RqK3%2Bp89EFiZK5zogZwbWoQdnLbW3zttlkoLhQMbwR%2FZgIAtJLWrb9VH24cYKSxYlt5Hgo3wkukgAjf4HgEN2PwXRwHZ%2F4jYmC0N89K2yF3in1zG5K9RX1bow5cdNkTEqNlXn0TBzX1NlhrSyUYOfdoJvnpZ6yzqGQt6RGSY4HD6BkRsWhWAU%2FN09nKoay%2Bdc33ml09Hi7Rm8NS46opzxAznJcxIq%2BvFjpvk7mz1I7X1oyO70%2BHpOf1354vmrhcAcrxCrQl6Mv8lwQePUOvSe5d8%2B5QSu3GIXfR2CvzTHHP4eHVcG%2FP3zAThfURBqdjjVaDF%2FC0RIr3G22XJ1OcNYA49d8OcjeS9R7ER0jbgfyv9eUYVFaSHjdb%2FIx9Mv%2FzmGnk5%2FuoxMmXP7JKRfMKw5Z2DVZrYtWD48Uuic4iZk41x0Yjm4WT1Su8YZaDS6FpuVmmK7IJDt2i5FWlgWp7gsqWi%2BdhYQQAZ%2B6xs4PRV0Kpdb4oD4N7qdzXgR6rgDyQOOSbCCfrdO4AD%2FEhOSapG2GZLF7g13df%2BgaRNZm6lMRILeZfjdr1on%2BvvVkQX6AU2rahQmvHjJHqVnA86906PQenfr0gO0H7tUTpT3ThCGMIeJ7cgGOqUBW6Tq%2BL6JdGpq%2Bd0Htqqs0sRIeJfWUAYl8h1bQqI%2FK1lAGezDPUpNf785HsNP8i2n7WNaSgbcLObhqQchbJxf57BHu%2Fh4qd82%2B6BMWumaKB0OLOm0kT3VZTkrvxO%2BHbMoWjAf7Q8oLbjwkJUZs%2BhpXJLJj8v9C4VCv7osX14sBctlrYniB77%2BzfGLskdy%2FpML5SnPBYsYUZ21WEbzbj8qxFUfPTHw&X-Amz-Signature=1dfa4292014eb84da9e5298c2d1ad64e3351a7620b5da438d014600ea33eee2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
