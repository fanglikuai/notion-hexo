---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SMRTDH6K%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqZw6KZEmtNkx0xVFzYpm1MCyN8D0DjO%2F%2FmU7JBcUUaAIgeQfmJrC4SKR9raoFWaaQ0SuLg2ELAm7xBtpKWTNMn7kq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDPUiASJQp1OXz53w3CrcA38DODqdcPZq1HO3sPmdfjHiUpykWTH5MgWvhvp8qRavTukc7EEcVFn7bClUS7K4Nd42uBMfn%2BqE1ABdht%2FlHnyZ0n%2FJUZggnPU8hcbgK2%2B%2Fmb%2FyPHJMmoZDvbwUF0JVXwm%2F7JrNRmHxkOEyAJ2Bt%2BE58criEbryoeeJrcitewLGjvqiII9f5K1irmX%2FzqzLQzuOHb9NznV8ygjQ2CL%2FCGhQNyqEg1M929vYN37SMkupB6keYDyBCN7DJlTMOPTqdkPRdvwovJAP%2FOYhtNwI7Ayj39UEOc6JuG6FJu%2Fvc7%2FVblcdDbAq6%2Fop3QP9azy7vTOwZAdapBOFJZxj%2FCQU%2BWLl2ah%2BTsXCVF0KfMmI1DtgnVRqSN5h5K6bz516KZhjTi78AYx991mVlSO%2FTkplHaVSksVBnf%2Bqnq%2BSHbFW4lcI4J2beGpTxTeCOdlKHH%2FokJrwBJN93%2FKemhEJuEWxmdzHLFYOjV%2FQ0H8M2sPOyI2RVq9N%2Bpg56KR7Nat4mWEpMLGxFYtCFO7kB%2FcN7PGJpCkbkOc5cQusvOHSoMel4JEkZKEWzrjCAqiTMvT3jHm6Sp6TJLWHOLxYbvbX3%2F%2FQj2AVIDd%2Fn6pKuzQoRCjDzgR%2F9xuinjTcs4R24iVHMPSX0MYGOqUBstakHI1O5htNayXl38vc%2FZwAUrsOeLjFFufwoYo%2FtfzLGa5aIti442ZmVF3gZ7kjC8l0fJyY9cv%2Fg03u9zKY6mv1TJdrcXwN7rSVBAzS6DtC2UAah5do81l7D%2FhhW4hJehTu3SJEnMJ%2FNSpJvt0xYTDukLoy4MMBU2AmAiaxQk57q%2BGWf13y4P75RHCMK%2B2%2FqnzMcKySHGtqUyBCyRb%2FguYuU9Md&X-Amz-Signature=60404c462ed40005eee87e57e166adb56c8080387bb0182ab6d8604b69ad7150&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
