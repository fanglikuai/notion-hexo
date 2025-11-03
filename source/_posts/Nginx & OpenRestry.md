---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXTDXBXC%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDJwT2%2F0l4LN%2FCVRALZG%2FKQGULC1Ui7rZBL9BUgqWORVAIgDa%2FalpNVngXH416Cz3vT%2BVSzsytF%2F08QUEhLYFRCN8sq%2FwMIVhAAGgw2Mzc0MjMxODM4MDUiDF1XdEBUAeI0KezWSyrcA6vOVGkM0OqTOLZj3hRHQnd8yh0z9fd1gXzyqmmCvMQRda0WAJRoXY3uQVYQQDWKPHlRuBOWghjqfBOTrNIU%2F3Q6jtXgCfQSjqonbkLwDAUz2Jnypz0aQ76S3QXnTYk3k1zKF9iuELPxIOQp%2BlSig8aUXyJNd058IElCBoJVjV8TOpGa2RxU9B0ppuTTKdDBMM2J11%2FuqUgNzFgEWicjfq%2Fe7ua5tRIrPng8KLw43r9ckcdWvcsFfFt6jQNUaQIQh6l5%2Bu1Tuxp1YT1PfCrzWV3yZIKIQ%2FgObubJ%2FhJ%2Bt1EqVRoShHKXzHZIagXnbJbSerbyDNQ9mn8gCnpQXtBnLrJHltyV%2B92A0guN9FBBxEyTT3Cw2JkKfg0sKVXi761X753NtxtgHBU%2F1E5DnHKKF3LFWSMNE3JUj61I79GPIemPxMVAU%2FVtizOvNkkToIXRmXuU9p8l3MKgEspwqAC%2B2BNVg3BIKy7gEl%2FsY8eZw9RAUmSS%2B4XULLZCCpKtOMzvlgleOxxstEJsPdNQMqHcPQp3RdTDPWE6D2GvqjkY41rhWMrn9C0nI%2F728SCnW1u8lK4MnrNwnIJZfHWUuDm7b7vkW5ZrztK9yBCcviyDomtm1seR5dAs7Qwa9PcSMMjnoMgGOqUBw%2BrFeeaS0GqF50VhMUZKIW4CDj8irwedOZzos%2FesEZvjytSTMS6BWW2%2BgkH7uil48Bz9HHN%2BfHG6tU9sllZyce%2FBknMXOMNSeS7KWZ34ubjYlBhndwWn%2FierHsOTDjERT%2FfVp1YSqt%2FV8L%2BeXPYdQZoJIjZrIW8rkMLubqshG5AVqom5JAFvSWmV4xCfGikTiWEJqfgCLQsYLLp8Si4PZ1yiH9FJ&X-Amz-Signature=43f0e333341aee9b5ecc468af299411546bfd25b0e0a107a2c013569f1c235ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
