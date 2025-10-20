---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3QK5TKV%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCIFuqSsBDCBc9%2FhWpUsaAsTD7XOuhCeu4RfG8WPwPmzGwAiEAgMdgWuGiOJV%2FSaKFwc3MdSpAUO8%2Bw3a7hwZIPaYGpncqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDoYQMWGfWiA7IwWZyrcA0Ni00TGWRwdZwHk5T308SyIjGAbvmjBIYSK9MOrdZFTqnShhURdGpYFZ3HIsAe87U5kc%2BE9JHFF6Ld%2FPwSzToqDeM4h3bf7V26jLDpYpwgPaB2kiuX5%2Bhe2%2BMfpSoYZO0WjqjqQ5uozbmG5OLRn4s8zD%2FeUmxsPhd1mIbcxrJfsgi%2BicFJf6EqAS8YD1mJq28bG9wJkSj%2BdC4njPPwiK869UvqNrWy4l9PflZmMyCwQ%2BnXFZFXq5uOLdSgVvsG8qeZTpUIqQxwDZZtXnNJJDxpO%2BEL2poesaqUgoMRNDIhNptv8yh142vHwSyHQs98sc7XwzsN5P1DH4za1t03QvBSofeVC7AOcy7EzccUV0WVEBVjlIrD282yuV81n%2BAbgK6K2H96lLRtPjL%2ByPz%2BrkrkMkwAwKOuLf5ceGL2Pjynw14hPWZD4KqLUw%2BQINqD%2FScY3tkN28NQusditW6rp%2BMfQ2SB6Q8yefCJSH7GSuwSQiPSE%2BCHrIViHxjVmw9hngPZYrUhqttq4c%2FH0FATW%2F%2F6XLlwj2nf7LAXB5G5D79HcXP9nZsGG3ZRzTOclEZxgfx2YvGuukmaJPq8YunzDoJxGqqb3Yi%2BS9MA0r1NMW6oXMH2aXsf6XF1DtkTaMLWc2scGOqUB4585T4lvUwqqqHThQ6S1MxyWyagbRZ940jU9q6TWePswPZURJD6IgL5nDEmkBgcKmgTwcw2KA5PLbTfPtLPUyA3Q5TWHJMlXoJx%2F%2BSulPXirEsBodK%2BMzf1%2Bl%2Bg9I1eoN0SE48UVez%2FQgb%2ByDYYKw2Jlks103QfxVY0jU8fjpHD10ilUedW2M7VBfF4S3EFqz8KrVhfUJGYw9xOHV4kF9Adz7muG&X-Amz-Signature=331b6cc9f1581cff35923131330beb68e072bc6cedc0d3730684a7a9679d1a24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
