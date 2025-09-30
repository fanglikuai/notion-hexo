---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TLT24GYK%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJHMEUCIQCXTW7OrAWsMwGPe8lfJVo6Dy%2Fsk6nJeRURFiLXEEokUgIgVVTSgFlILecSWgmsfvndw6OVRb3zFteo9yMYzfvWZh8qiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNvAgAeMEO1cp9ryJircA%2BfA1w7AJcxYjFZqvlNNW%2FBs6F8kvbZEhKudjq22km9AVjd%2BZo1r9mJOYqlLD4zoKhjpPqZ6uy82C%2FuY9p8iET9TsVJYV54uNyDpxBuhTHCkhDhPDQn%2FGeXn%2FBJFb8P0ZGdk8UjMD14VZDh8eRnz3QBOsFx1ziRajFUMbylchA6BkVrj2gJZ6Hi2L2OdzaSdahOcmn43koqYYo83QqKdMe4bqQ2KeQMbQZ7mj1i6x%2BKJ2FXfAG9GPTfLN0qh%2F3skdiCHIGAYG3aEFyEPCDnQ%2FxtoFlBJw5Uy6Ub3FEKsKgIhquUqyc%2BxIfDObmJ5%2Bvm5SPHeO%2FLRD%2FhmMbQBLQakH18US9Z1abReFQRjyTM%2BtcfeQktVhL79skr2b85ZF05%2FvLu4Oogqrs4U2CkHaLaxPprt1mp4x2L%2FPPOSwXmWWNY69M%2BbM2xZQp2g2eQ9UUfF77JHzpnVe9bZtUkdaf%2B5LwjHARbF%2FfG8VgDLTo09XnPbWXTP%2F5WqaJGEEoijMh2aXOXMGEqJwfIfM24FANC8Dt42MXMuonYE%2BbT0V30XnvyB6J6JCDEg2L75fJCKRoKgjjekwpM7CGeEzlGCO1kfWCABAAz3LQ4YeCaTyrfg3VXMWT6ePTBs1EB2LjofMLuf8cYGOqUBzVCjlaL2I20giFaHrwsQP4t6MkjXdtOGMz8XfU8iwS%2Flxa8qJPSrMvAMMRfxHZutqfXd3OZRo5ztgCogiZvUmChkd0tsd6iCdkHAF5a7XNOOcUviGfEoHc8gJ8w5WEAMebTJ%2BpsSWDtb2rNiz2zJzbSsveLU7LSXBAAIg9Lenb5t6stmqvVxlBAn6qdW7gYfh60DaWWYd%2Bx%2FgopLeCHvw8ciJxmU&X-Amz-Signature=a7186064a706ed4f1b847d9204fb8573e381e72917ea0e678ab1d0eaa95c719b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
