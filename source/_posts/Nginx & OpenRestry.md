---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QG76GBAX%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAU1uZ0IaJEf2MfgKuSfYPIbr%2FE5%2BU2gb7GqHJtzIjAAAiEA6j%2B6IwQntTGjrOpycOB%2F8UEIN%2BUlpBPtUV8PrRiZB20qiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMjmeAPw3MNm9m82circA9WESGs7i0AdEFdPB7nOVTP8CK%2FYhVxU9Cy1EpW9b3%2BAh4AmQKDmYFGoOSADt9u%2BgDmF9ib94%2BYJvhljoSC9GA%2F7ZrvA0KPFm8%2BXQbYWVXaws1B1RfAn0GuRBiO9jh%2BAU0UBuRF0MOu2EwKxZzZpKPnlJbw7LBMNpm0pjtCnkNKJMSwIAoG42YHF59RM9acn48UBBAduhBpT7INmCxGlQEWZ9TNLtlupBiaZSjM21LnQKIDFu6i%2B4xQLK41RaUt7AIW4pQZYyQsnJXlMudfc8hTP2aWu2LGOcAp0uE5oQg4rxfdxxqJxxOZ7IFg4yDAe%2FweoIRPgQvCRIvZpTsEZqpnvdp4KqxFD4X%2FELfFS8DthxhZLn9ywGGjezggLb6UAHHBxnQQyMYb0QTTCDeRJCCzyypK9R5YC3rO53blIa9DCGmdBk77aVy86O7bex3CTI69Ib1xHCthgeJ8%2FvM1pOFtkj3Z6mSlnVDKtl7yHVM2dyURTZRfU2Kih80JT8cCLY9adtIcujXIY9fMRKd8vEgJhDDb9bKOzNdlalw1ZAybORGZ3%2BPxuZgTi4ZbiimGJMoHzxaQrxa3UKw8oL7VK8aZYVvkMS0kbX4HT7j0hMnuL%2BKzfCQPaToMvnp62MKb%2B7cgGOqUBGZNLcPZjl%2B8hviQCQXksfu7f1nHdt8EM%2B2AjC%2BcxdJ7X7RwyvJLbnAYjRAsnWFJiak88gZw5%2F9XXRjOJdY9ojFy%2FqNvRXnv6zAbwbWCP85ohywRIoMxQ%2BNn0Xk9Cuw2HWbbBGwc7TFWaFUqZ%2FxfgusbRc9rTca9Y6%2BgqSDUmKNumZsLXI%2BcOJNrk6zqkNeOYMy0oNoUOWGYqSd5m2OCXw5o3%2FAUS&X-Amz-Signature=60141985124f8bad9991272b55ec9183dc881c98a1b010fd586f91f7a3bca21b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
