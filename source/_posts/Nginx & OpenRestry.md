---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664AD6ZWFG%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD2TXH64zO3FsLp3V9PzSDM6wl%2BrUz3ll2oEj9YBg1LiQIgW2nojr6IUzQ8oAL28Zv4dujF%2F53NzWsQ3ZaSPTZfPKsqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF0fiGoJBW5c4evPYCrcA%2FxZT0Q2MaDkmgVJwsVPW3w76KF9aZFS5OGNLGWk4K%2BiirSO%2FdslKoZsanOTe2HxE0pf%2FGbePiJjjicYDckwQSgSbY%2BLc4t9uT1SW5S4uGvwhHrVXzBOkhVmkbAQdhk1YYJWkw6yB1Eyx%2F5yMgfvHpsDX7FEgKkCdyKVKj6OCf6NkI5l0zufovHenkf8W6BJLL9D8BAMb%2FAcj5iLka0Av0AoChoFHZoeWy58QyphkBknee3eeqy7iCdOpZLskp6p6fTc1jJQYc1UbfWnowTVepiBPgF7CdRaU5ER5TpmFHEsOFoLMZ3qtXljQYZoL7%2Bifw01sDVRi6VXzga4VN4%2FczrTOTseywxCsXJL21j6J9WcDRYlZ3WkknPuLCGImY9yplkVDJcLW0kBgk5p26PdGPFnqx8V4nmUAgfAmMidS%2BzyGb5ji9cQqV2uIp7Bb0J2YSAhzucZ4BmIcYsc%2FlBHtaL7qXxSjVEHVKnnNGhHwoe0ogOM9BNv5%2FjfYzdg6gQllJ510hNRcWoHCO0mFNA746T6xJF57uqcDNE0pr7qHauivsKiIoRB2kv0lOWyyQZ3%2BwGsDJVrmL9rYcVFKzB%2BGFCcZaUBMGwOADUPfgfsBYKnPdiSDYPfYTR2mTeBMLXBxscGOqUBhArngI6ixQvhOAibVngJUHyA04yxal%2Blp4xHN3Aity7GIyE9scvQABPQkhsEFZyORKRucHJgomMToSxaVwT89FPRJhUsJAD40057ySrnanm%2B38GnfK%2Bbbxci%2Byyj1sarGhhkcM524g5aygKH2adFxrzOUDtUqbckXAUnTbM0oJXwyfvCQzuXKu%2FGxQ9DKQlME1OfPyV43%2ByjjITm95rsOvadrlzJ&X-Amz-Signature=4f1e9299c1c0f26217d2696dd65fe01f0391aef8f5962593be740f3dbcb8f2b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
