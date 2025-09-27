---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663X4RO5M7%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJIMEYCIQCWTY0spzbyCjvTmHC5eXOXrpImZ%2FlQ9Sa9kbD%2FTnNmDAIhANtaj%2Fjr1%2FQG0zWSrfhwktgwCJQakKmSW5ocrbCmFLSnKogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy6qDmfLIeFvFho1Vwq3ANhTlxuorgvrwjBJ6VQXH5xUx4wZLWiA8AN6AID%2FrCDrVGD76KYZs5vXMvMzuhtxsNqpLux%2Bf8pgs9Bvnx51KxwCqyQoWo6B2DV%2F%2BVXlvPxGmIgVkf1K2QImkwBuuDlRjxgL7037GFJ87ZVyOi1bgwzH%2Fj72k4omtT3kg2CA2Dd7FL65OIWp6hzuBlyNKS%2Bzuvgv4jGKODu%2BQ8rZPMfBSEtclABvn40oGv5oNzfyoSXOQKzeTM6XxKGQbHKxYy8%2F%2BJ%2Fy%2BXTwgfnpj48vDt9k15m4jrnxegOLC6I%2FvwXg0zB%2FXKuMeVQAlAOWP%2FA4Z6zuOeUS%2FvIAGuU48PbzIZbTgV9WgSF1EqJnZtGkpbKi974TOFqrsj16IW4FEi4u5SU2k%2FQcGbiZL%2BnjM8t3BT0T5CF4Cs57AHNT30xX%2Bu%2F9iTykCmPLJRndHyZxQvD0kJ2Rk9dtzEFH6COINsB8WNNlR5HQegSueDB4HDNJ3gpItxpuh%2BgZuvf3s6Z6pWAUD6QVjCd5DreiLmwarhZEvWeN9l28d8uN20zT%2Bwjrz5samRwVGSFFYghcrJDZMVzoAPyYvYJAa02ycrgdRcOLZQWfYHlFFFFYIm1X9INA%2FIkadlDIxX%2Fdm9cCCL1sApuvjDggeDGBjqkARIxhl2tsOxiMavUFVnOBipoVRJLrxY3kEc1V4PW%2Fq5vynwhkTc2yx9SHeIqQxdXZlOwyVKEk4Ls8734C0D9ke%2FqrHGo4M9Qe9qDYouKoWk7sMH1SToMvTP%2Fapzi2j8J0yhHqWVqorQd8sXNfzaKhSRqGWqUnDXpfWvpPFHaTLGHPuMRfmXYZIWeGXaAIiEEpWsYQnznZ0i6pEwDAsDMVhJRWcB4&X-Amz-Signature=e2645ea6f376475aa8f2efa4bfb2fe5525f9599a02be4d3cdb5c01d1f0104547&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
