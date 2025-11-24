---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OH5X4KM%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T210054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICSOnr8dchXqynrdLKafOZYh%2FQiHadANCuYTFaWR8iLKAiEA%2BTwAQb9V8A%2FvR93KdAlLr6LvJhjxi4PqVYf5iZWbuSYq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDIJMMQu%2Bsqf6KiNpASrcA0uUNFBGgOVR28KyXKz5WHscKbp2%2B8kWzPXrRZtQuo%2BHPdsKDsgdPH1vnYQ7vJye8fNaMFh%2BcSegwOcr8Q7G%2FL7Kry3HN8LFfzBsS%2FKYPN0jxmV6iAxEt1sJfl%2FCG3hcxnWhqMpIY1ANmCmjXUUtvBmhfLVfZNyAIMJ6yGzjrZ%2FDOMCjQ5xuRRdsP7zMCybmlc19zwSUa7KWRCGlLFWgoMiaDJK3SZvlzW1Nn0Vl1qAvGr5cxtrFdGnP6TcntEVDxWhVBNqPx1zcCosfiwURm%2BtCJdWbOkfuSYneL4KHwTrFXLL3cjezS1OUEzljhnNP9AvdxMajbqZceVJc9%2BSjoCEm2JBcJ3wdgNV36QClWBmk7jFvAnMp6ZDQfubRYacunIKXxN3gAbv4J5cwSi0GuKJVAkGt3Kr%2F%2BS4xfpCApV46BO%2BblpMRGUhWE3%2FZxVCNHz5qVA5ITnja2c8iQL2%2BVUrBKZJ2pLUNfTBJQvPeHsdrEchMB%2BVsrxrWMA1JdjvsDN0Lzr%2BREIdlYILtPDlXMG6uSGRjhSHFZ4xxRxnG3OgeJDBUHB5ts5AwywJI2F7Hn4wWxrfu0HVbr8Ee4PBeA6osndj7JUrJu230CpstACdOqtSnQcvJRwygweuJMLH2kskGOqUBOOwN5y%2FNag%2FENjuuLwex%2BCeAQG2X0PhoIxT0vtU2Bu39ZzErjTOSgMvaD75pnSKl428H8xkBY%2FXo1m9sj%2F6HJOBUOVwo0UflXMxS8aUtNmcL%2FMlUBzS1YT6g58TrJ0fhlesZrq%2Fv2dd3HPsvK%2FCqYe%2FFPfhpDlGSA3VG8ftxMfMoHLoan%2FZgtTA3tcfcHLNXt2uYFVrAGNsq3LhBi%2FlOihriWr2K&X-Amz-Signature=bad260947ecb4a88aeb48304ce2bd505465bef6e666191c96eb161477f7c3390&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
