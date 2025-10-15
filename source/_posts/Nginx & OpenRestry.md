---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGDZ4W3M%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDO9rDUL4N1AhBbnbNikk3lL29ZS3p86UDxHgomhoOMuAiEAgLjvrl1q0J6npNsvSVXytj4uxT5RAusmJFSF9DumMLQq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDF%2FGfhPYVprfvHwplircA98Ry%2F4Mur2Bhz2Ehro9heoht2urMltYexJ1H%2BS%2BWtFDShPybcCwCcmNyz248KZ%2B3ZTlyJ6pJ83XxY4YND6XvKPPKGidzb1mvH7u7xmTtKyU6Y2KcVsXsDtz5bGawNVZRfKm%2BkRfT9YPqbJYITH%2FkL4pXfkz%2FSvN1jxlehG77LxW9mjyWvXzwpemypSEycEJXOCFBss%2BKO%2B3cVimY64cRQ5SsvMnswECxgQcI%2FfCqDlIbAYhg8GDo0R4ezMrzD4kWq7KohTXQEEQQErICrzFl1T7fjYjZo09RU%2FfLEBSWwcaKfls0Yhw6fenFOj3irxytctH0%2BCdRxE26sP95Rr5dAMv7iW9Of6Yo7Cl0KltpOTIgoq99%2BF4Q3sRYSp25PyJgdlc%2BnZEMf1IySqu2%2FiOO1FLaKFXRJXVOuyn%2FZWLLR7pKllKFFO1q0i59XIEO%2Bv2ePO9cehPaWgQ9BEzeC3zmtolqiTiD%2FJ9MzGFGKYE%2FVe%2BGAMFerLmer4Ty7xRHp8eDuGNFiEPBgfyKRa1wd8zxYoZBkba1ypbTRBRvolhlcpU67stl%2Bweqb36gNtuoeEdxaoi1557BGhPCF99Ga1PJHNS2yHGqVi836M6PQvaoVlaC1M4DwA6D%2FBnkTVYMOiYvscGOqUBTjDfl7YPYIPZxwDuKDL%2BAZgUsHjqSeWMUNmDbwHQVTob9t9HpkIpkpzevjqScGQPr3s8qiZ%2FOfMszg8SMVSP6vH570hgQIizeIb%2B7nf9K5JyNFBE2dpT%2BZNEaPDdxC9eqgIA3e0ktWwruUupskmK0JdLJdC%2BJ%2FEaIZv5hQWAKyXbdCpJZNr1L91w8o%2BXfd9t9gqMy2kd49ohc3w4pmWQxSlo3pHb&X-Amz-Signature=20741e61bd43196dd422cfb0096a6572abf87bb5f41ff7ca225189e9e7065dff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
