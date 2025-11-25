---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ZPEOUIV%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCI0BSyGKOSo5tKQVGaE6yMSRtLDc2u4M6uoscsaFJfxwIgBKbISRGICGbY%2FctFDAoE2mbikbIM5xzahR%2BOvXjGGVsq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDEjJQlduw%2Fiz1BuyoSrcA3MSzaNUNCWh5Qsert%2BZI%2FlTVQm8Nc6NvBnGVHZnwJZj30vYiX2yciwkW%2FNBAnCk9M%2BOj2V1wTxb%2BMfp3rFWjEKnoESMAEkX5OFi9%2BbefEdQMOArt1kEAU0uuOvwcDXk81L0hZlL%2BOnsr2LNHKTUPFm8kM1NMyYbKMsNIcui2WGEGa0psTUnd0DhD%2Fk2kmMbedleo%2BT2AXTzgwMcNkm64W5FDdrpQ9RPULYTJw5ZrLiXRhD5OwwegSj9MhcK0no%2FERk0cTSwehNlyGBNAbPqfNKxMXgY2s%2FaR6ibAq01kJKBoB26XyYLD1xYfhUrEowK6yN7wfPbj7mVuO9ugYVB1ebW4C2GG8EW37bqv8Ss%2BsqK1div41SA7D2a2Cpo66N4FnH6sdmxpVOKlIRoH%2B0BQbA1H%2BdcHE2HbPrnVib3Sz%2BcJ5DKOGr%2FjvsocpccV2EB3q46hPaxCNICaNJD1Dw9qt4Tz2QEFy13QM9%2BMy8ENrwtDqkvtxCpjQ04RkS5frk%2Fzzi6B3waDF7cvLdNdZaQfDqRxjtDlzHipJFx%2B9kuhVlQkZ3tRZDCE4WX%2F9QwJA3zrHob%2F8%2Bcd2q1pBZrkN7zlBysqoTbogCl7B6But%2Fb30TUI1s%2BHHZ%2FdBU0Ebm6MJaalMkGOqUBaVdl%2BrLW7DV1gJM0JohCUyOMF5qCsW0j3t%2FJ1ENr0z0znIZwI97IlzeDCtAni8%2Fi%2BxX4icxKPgzrcPgAhP5JczBNJwXEQOUPznDOW7v6yaSNClqUg8ZRqUCbFOz%2BLsFBKCTUIWlq5VHgVnFA4XddZqnw7Q8DB02fNy%2FvNPcqP%2F8wsj6q9emelhBsTM3YwT5KyJTizsaIaOn5DOE%2F%2BCHnD9Zh8mKF&X-Amz-Signature=b1ad542e498f8286bfc744a69156fff6a7b6b63da938eb65a9731b9ffd8aeff9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
