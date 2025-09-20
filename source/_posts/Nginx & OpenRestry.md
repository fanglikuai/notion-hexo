---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUU5UFGX%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJIMEYCIQDBTZ5a5YjHxPc5CALr6lDoK2BpbOHN3%2BwhUfwXxDdkvAIhAPeX7GRkNYlej2f%2B6UkxSisOkUe6sT6gwTi6Efn6T6TYKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwoBp9a34nvTqkIbNMq3APdUBeaexrHilB%2FMNxiB2w%2BRUWorS2zdH%2F3ImxATOXMbiEgGabXVE1c1cL%2FCpT3lr%2Fd04JNQU2e4JoRtmmcejeKBQNLjgqt%2Bc1CjpKm4%2B0qltW28Ej7pOuQxOO1hlzayjp0g3Hze%2FeQ0WWdMD2sQJdLIdKj%2BW5uH0Ni1eOtITs7XSetkcwm4AgZpGSycRyBpBRmWASbEI3swNwJd4M%2FD4YTU%2Fuud%2BL9ErAu5guD0s8yLkTk%2FN7yBHpMFrbnKnQoRjz6jvOCe%2FjdjrxKqO%2Fwt3eqra4Bxxh9cShdYN7gbKrtkWTqFYxIjnypO7d5FzIgKKUIDahsZPrhR4TybVxSCnWnJxscIifCLok3MLLKwMr7OQvCe%2BKzdaLWjx0%2BrBj6rm5rrZvvvG3zJmRjEmnMCQEQOaXMg288w0C%2Bx%2BeYyMLOuaMmObejRUtiQkB%2B5jRT%2FQMJ%2FYXacseYjvrD0wapGEUizhjn6gAmEt6lXPV9EJgu5qJu26kuMHMCD0NyQEpyCwUPKBROHohSsfPPePoSLMCYrW5SojYPW7Cfyk1HTIfEMPUqm6Q8Hw7CBExI2066fHN4g3eZk26QP3FfaTKu0CgDfhQlIwz4K%2F2vE4RK%2FuICVdRk3faY9Duc4OPuRDCv6rnGBjqkAbsMETn1apvkCWd1c7hSHofM86fZAkvjQjvC3%2BqVv653kjQXIMYRbIaseGmrKgcEsxnP1r1lScVwK1sISqTqn1flCrQOOsTG1NGREDmNAs4U5n5pQTaoJbGskR%2FwlQx%2B4Tm9u%2BEbXQAAEyk9bGlW7%2Fq%2BBr%2FsB7foCwOTakNrHLuuLbN%2FDTmzhoXGDZ2s%2FKifn4tFaVgmpAG3kQv7JI7xSeWabQAO&X-Amz-Signature=734775a1678116cc8359a35055026636a08189ec52f2c13273acd7c518907580&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
