---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LOI337P%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQC%2FEWawZekO4cSy14Xfl0fdnGO%2B5nHLGKYbFWtNYKVs3AIgWEALIjHN9m96DDKlCSa8r4IWNNPWp%2Bqyf5BPtZIpTggqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLYbzgdCCxkUSnKQQSrcA%2FtofZSPGcIYe9HgoUvk9EQ3A%2FMP2%2B95YCkpOrlCFB71PCPC8dgb0VAE65invOibAVRUmTDBNADYzq5H7yPFx23QE2w4LzkeSrcPTJ%2FLA%2B9nyUuHvk3i4k7CTAiksiisnzdZfJt9IMcCzGPBwivFkGpZP1ms4jeAiKSXHOR86IkQKvaS5MFoj%2FFbqVMhmSjkX%2B1y%2BamOA4oqrNQGwjgLZDlfPIsQIg9CJ7IHk%2F8jslBumVt15%2B79gGxyRy%2BnqXOJYQ3TYIOeffjJV%2BeP1DSkMHMriyUb9iJ6svP1rSbMViHGqKwbbFDOKQXGBygAZz53ZbxMZ5Z1MTMz2%2FfQ3UhSpsC3mjxYitR3BxUzHTXmGsdFHTCdtiCpvWqlyxnxk7LEi4cboesoAXxblhoOQxAX7libEnAb9ap0%2FMw8rSLJzIhSvGfaRTnkWSRiE7Sww1AL%2B5tFP3C5nqqRNkSy7Obmb602xlBczd4sv8WYogaHup5vKun1Z6SQH6WwUF8tDKxhHygieqD8WVuv%2FSKjUmFeLtR8hksOIt3z6NQaGkFEsOXSFiNBUOZN6WK%2FejqU%2FaqCwGq%2FlgQjmWi%2FAGWpBFoS04umc0rzYty2dcijr0%2BLrthnM8zP6G6pddHaqBJUMIDuv8gGOqUBsuYFmSqsPaflKCQ1A6fd4OT1D7xVgQtYuL8apPRqOYIJMZYNrcjOB8HAGJocegsNcETTYjglwgdOHvcl42I5PdyipOCgV0gUOf%2FOKc6Yu%2BguRsSU6W%2Bvs1Ttb1eS%2F%2Bbla3QTiX2xReto%2BSzhJVdfX2eTKo0fvQ8QO%2B%2BknbgzQdQ4WQH5Xh6zSXIqtf9Q8Pb8Eky1Lmrv0UMCePghxKBY9IaS5R7A&X-Amz-Signature=afbae66b62de1de5587b61ff6ca2b4649acd6802bc062c1903a2cd70aaae6c66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
