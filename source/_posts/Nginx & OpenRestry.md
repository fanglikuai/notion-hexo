---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZSGLFYE%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T140054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQDmwfW0d%2BjI3dTYXgtECfDlNX%2Fk5sLDlO%2BuqY4TC0WkmgIhAJ4IE3bnABWecfKKkyCc%2FzZRSKG%2BBpyWDYLjfltfTLW5Kv8DCBYQABoMNjM3NDIzMTgzODA1IgxObz8JREjfLsKGSDgq3AMHxTQukSlRLbdsXZUP645Z3v9vHnJw53tbf%2BUyVd9LGxvZEgqxe53NvrsjOElkTmaZYrN5W6RUonLEfYkU8lxz5qLJrAEVvX4hKzBGZGyH1xmRPUXybxc6yYy%2B7WFiA%2BkBNI4Epp%2B7jNgtgcI83vE6n386VUcBc8zI2kO3n%2Fa%2BnvfIm%2BBxdTpCiOfBCaqFN6UpfZEgEC73f7ZNxqCuImsKQUX24jDBmG%2F4WEZX07HTjvT2kIjEAOEGwEO3Lm%2FFD9xfGviw2xNNwwkgIlIZ1RcFt2tXAqVYU5klHARgIbnl6m03UzX6b4n4nd5Yc2uwb5a9o3tuVO%2BBoIgBTyR4QDrobd0N%2BSi4IIKhoRQRYugqW0NnkOFbpmGGhXJw8VItFuO9oI%2BgiFcfkES1b2vG16M%2FM%2BhX6CLCPLXtYcVJHO7ADt9o92Wjbqe9O4u7y%2F5sH08zq22BQawBwR7rsszwY5bmGkVn6Zwm03MM0UxcTlkoBG4Iiraf4SmDf1jCBMZV88NXcBBPuQgriNB8FUk1B63dJLuM1Ago7xZbxw6ppEpZnxiZX%2F0JroUI7jNNmPEH%2BBA1x2gtPRFWvebbnofynGfJ6E8t1S7YkCpTUlNt4i%2FdSHe0Z085Ok61%2FPNkHTCN%2BN3HBjqkASIBIHiMwwJ2Y%2Bq02MvCo210LtldMvDklP5nu%2BDD2kgelBXwmwS76KXr2btOzk3E3Syvc%2BRWGVmeiTpzaoVQAJqP8z5p7NkqJvMx2tbkuPL%2Bf%2FAKjIqgLeSE8UJuX8toZuPWBzvUazHTffvI0gFrww507uRKFXqXdZYiTqzIr08imzJDvHbo%2BzGLjvCgKHCCrQiAOn9AcZxCgR40G6OyG%2Bf8FPS9&X-Amz-Signature=a2c291e6f8bc42cc9c29f09614c5429768a0ecdd393504bc293a69e46a5ede73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
