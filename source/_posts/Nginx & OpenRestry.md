---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664K2ZFEN2%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIH7vDA%2BAfpg%2FOYsuI2IZmH7e6l%2BveKG0BE7Z6DbmtcTpAiEAgJ32s%2FICRf%2BgYVqFNSGdDPKZz%2Fyqm3N%2Bz1fij7Ld4ZcqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKFXneFuaYMYmwqmtyrcAyQab8Lo3ma%2FdKZj6BoMk7y5D9Iz9xk4GrlSV32SPPZqxEPdqTPZhxJm847pjSkbO%2Br5fd9puL8My67uouE9WztWlFl1YIpqk%2BEvv7vsTNLrt%2F%2BGtv8AJUWT%2BGUVyVq%2FRiPh%2Ba9Kkw8qd7vVQ%2B3mePE42so0daVJYl%2B1KXne2tsKJkLSpekItJATQOUMHT4wqgJovfAu2UC1TdsWzCOUu7MMn%2Bf8OthvVimc6KjxcK0gM6o0b53GreiKbfcHaBzGaF1eOdLVxQrLQ9hsST%2B16cBPQB54rN3Rl1Jhvast0fpa0c%2F0wxmGs2HtUWHWZaLnSd%2B0zQzKRSPGEXdHqdxduj%2BQz8FSBYLPlqdk4Eq9oiChS%2F6WG4Dt%2BCQWmPF0obCKgJE5S1ZfWh7fNar1qjdortxh%2FVbmPKmlPQWOBifRbm18QbWtps0qGXFlODL3ObWEA0a3MTE7q2kqro3OwD%2FRSbfWXoyx%2Buc%2BSI2MM%2FHqJmXHwzod%2FFYvh244oLd25%2FUEJQnTuqhE37vrYTAG1zH9DoDb1rDudfW2mPTRIekFhxLBNhZ%2FMsKiSOJGTzsqKr6PcJD2ZNhxVsebrcUjP0q1zJ8HbKyBH3RmXcVGqwaUMzkC6%2BllDO7Xmgy0hlUpMIjH8sgGOqUBE2CC1c3efXsdZvoJdvE33rrDQW3ycXX2U5pt4oWHKEFEo8EjQqm%2Fv1T1JnyGu3I02cdWqek0LzZOUBZTr1Sge%2B0jT8e0jAPsuEBSzTRP4mCnPCj9oR1xaRC7UdOusPu%2BcX0etEW64tMo5PHI1ZQ1L%2BWfxygFhDsysgTkWCm%2FyzM4IKi26Y78GlUVxmBTHKsGLoR80cxq%2Bd5omp13YBUKS8Bw0YpW&X-Amz-Signature=123b63b0f26d5b5b21bb53874b820922c9fae56f6b77aa036830583a6ffd4d7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
