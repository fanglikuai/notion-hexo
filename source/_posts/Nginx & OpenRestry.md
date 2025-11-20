---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYTDKBKZ%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECsaCXVzLXdlc3QtMiJGMEQCIBmi7QA5XgSUYNSVhPCPWChmLZp%2FvEzbsArvZvoQRbn4AiAojstQ9sx4kCXR%2BuAKQtgs%2FeRzrncTdGyabv08wsPkcSqIBAj0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEWj6B0ZdbABkeA%2FRKtwDLTK%2FfBLIu4Fbm3zcQsOwSh57nDzzD61zHajMI%2FjxUre00Jk2TGWcL5sOOqbTU4Vxm66vV%2Fu7JFqN%2FKUsJyagvct1PzWggfjEP2xtNaAO3ovO22mGH7M%2FAf%2B8Y5gbQom7OqAYGp5Xw9kwFhnOWwYG1yUMztoWPjYWG92yI5ub%2FS8anS3xA6MrVS9iJ2EXb2KEs2l%2Bs4xBdyct3srzBUDWKFY8erczFCLz6cJ2z62yB%2FSL4ZpKWXgAnmTZ9%2FdXZS7vHMQ35oogitbovyQz35lct28Y5QRViMxOVH8lVpzFVdNLTNg8rGv2%2FBbLUwcN6pEMiJGi%2FzWmB5mpAsEMVYD%2BNzD%2BdmFl2etFso5ecgXZsAhm034z3QgQ53jUP4xS1z25kDCa4C8Bpd%2BIrTSMzwT4%2BEN3QFxuvv4Y0Hfb02BcJ9LQIj0n7gJ4zZQmyxOMAJU3eNmqxeGBnmWVi4wpdp%2FJVgt36Z6FI6kHnJv%2BAflenIk7Qb7rYbAMShoiUHy4AZmuYrT1R%2BcwWXO%2FaWxT9Qudl48tV2gptwgzbhYDQSrgJeuAIlKQI71LOzaxmlocHqEiSCfqbEmPjneX8YW%2BT5SJCjXvKtNcEqYo%2BvOzE4z%2B%2FS%2Bm1SkHG%2ByKbf90%2BIEw9O37yAY6pgFeudeAebIN1I15ZnQgTK1usKguaT94zpPrTwyWrQVG9A5vuqwxN%2FcGe14eG5zbWrNJ2YbmxAwypuVun50ffUdqIFnrF7x%2BJCz39nk1fUyiCeiHqMKM0aqKp7kD%2BfJcHNUYv%2Be8MDlKKOB6NqaAE%2FEV6fTn%2FlgwTGFh6e%2BOTU6ihZihOc5cd7ZRLoJGYUDMWxhf%2BAYf8vSmz%2BJVo1WAjGFqeuGIbe2F&X-Amz-Signature=b5d11b559dc42944f4c864b3d02eb43a59d1f3cb84c87c87c67586712d3fd08e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
