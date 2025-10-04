---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653WPTHCA%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAoiRfPmElrUXaIb5YWQEenxG4e2Ss2anpt6SrvbrZujAiBOaInzAqVtCJEbdMEcJcLCevh6xGYPxgCKhit4MdAhpCr%2FAwhVEAAaDDYzNzQyMzE4MzgwNSIMoiSiu6a0FeF4RTKLKtwDj0RQyO1LCw%2FPMX6m93qPVu51AByG1Ml3CWph3ft5XFGB2vTmie7w7jaqQXVQqCddgXQ%2FYl%2FHSdxFRiahugosXgOSmZawyghZmGK0Bh10fGnZw6TNqPDhcZZxUMZMmGIbAv8MPgiD70UCFFJn%2F5hoC%2BIUiwnZLITrvqr1gd8NCVA09S1TF5Mz%2Fey9kThEEFCv34GSHB4nsi6xETeG7YZ%2FpoUVHQH2EwahdIsQ5f8euZJzpi5n%2BK3y4Ax%2B8yOtIYIoLOX6XKfTYKqJ1g8nlk1h2f4%2BqPVudyw%2FafyQGX7HVFcBYfg0%2Fz7ykg5qKIcp7IUMMJGrph78KKlTXyJnDbU6yt7G7lQyXPYYSel%2F9mUx5QVnLhScF2n1LkEMgL%2BM6qDLlD%2BW7CiN8lLwM2aFtZe%2BIECgIRwvQ362pUYpwdBnx%2BQys1QZCfyIsBSza9dHIhlbI%2BWqfxYT87DBryLNWcox4cZXFcwYgBrTMWVrc9V08HprPx8vnnkehoLF%2BdpofAzyuxgB8RnSLWUnbV1%2BvCtNZRGNzCSknWzG2w0IiqCa2um51wzZ0ARBL5lCSDgU7xe40T%2FMadhB370rAVBY0Th81IjVfL4af1ejn4Gj%2FNMLO2e3S9CEm4vg%2F5%2FffVUwvb%2BCxwY6pgGaUwMrCQ3qJA2hsCY0LTO5IjQQwgYx34b6fc%2FiW7sv%2BvDYQ1Fq4DdE5b44Rs5R%2BFoeSSkkxBX4rduUMv7U2krhgA5t66ua4wOBEm5ou%2BwgYjtePBcZadexUnpk7SsfW%2FmHFobroPsjeGEuXXibtIvzhaNlbzzMwEv2BRn8d2kEzB%2F%2Br0iAjaCBc6VgPNQnsA%2FU4Thm2Qgui7my0YIBJqskt0PwxnSU&X-Amz-Signature=434e0873f796a4d53227d5889898263ef2db2ae6f01c7a4c3363b47d4a8a8da3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
