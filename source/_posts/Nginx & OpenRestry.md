---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDWQBZK4%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJGMEQCIGsxXTEnxF4atQMxy4MiBnPBwk9EYaYgxFGovmfa%2Ft37AiB2%2FoaKvM4cq4%2B4xWqwFtQpnQk%2BmMzIMLTKe1FJCjg5bCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyUkThqFB1wlg6HOrKtwD12H%2BM2rq9GGQqk4gvBUzhs1gPH0K7RPd4MIgCoDVglUsJ%2Bwa0q8AWawbcU%2B0B7nzsqfLIaS49hHrHMFOcRzac0ofmhxej4jFRkekacKSt5u2wUdnR%2FMgORBpbkju%2F8hpF22GtZVX6ikTZhbxAVGaUvosOAdLicbA609hB89yESfR53cWVgN1fSSXuMf3bbeocw7XCWFrcq%2FXnzYMGxgHhP7T%2FxjZSe5uhWYbU8qXMJr3XjLPobtOprogVy94yTm2RR8qcrhoEGH1xm9i3Vd5qm22d7uuDuN3hsVNCAwpd7w0YBfIaE9NDlFFm7drBq9QlvDRLoA%2FsDGQchz66pML4jYxf4oQTl08hqFdfT0AJuLwpfzxcVDNegt%2F5DJpbpj03thn9uhK7xGB3nrz8gNAqQe%2FI0ffiaLNI%2BXoONjpIraPJR9qLDG4wE%2Bv9GXCULJyDc7MUdHG0JaUfvQgQenkZV%2Bn4viKVhyAfMrso1m7zJYY9y6VuRs3r9BgBcRmFRHr4rjNel7TXbOna3zSZwh5mRQMPrB7rFB4NpG8k0Pcvs9%2F6%2BCr%2BWQGvmm49jW4xBEtBNrFhLl6O0G%2B%2FyoxLoxkrYX6QAlWeDa6%2FnRfKJkvCJM%2FdRqIOQTsP2ydeqkwr%2Bq5xgY6pgEROY64sSUdIuAFBlPb8xZEzwPeii9u4dNsPzTnyGz0p7zmSOO9gmURrTWAboosNhcJ17OvZ5udgCrsfU%2BtD1kjLeHrf32T6C4DtG6uYu6kdn%2FF0tV2C2oyU8Ds6dBe8rE7AeHgnDEfBBnkHGdKa9eawIBRABoONO4Bk6NN6bKDPhq%2F%2FsmIXx%2FH7vAzbvRKXPBrAHTXFBfwvLZVthzleHcoaPUNah1C&X-Amz-Signature=ac9a4adb8e9fe63ea622b31ac914d1fea593135c5ae679dde7b82d10ed44e1ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
