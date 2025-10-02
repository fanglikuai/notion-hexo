---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN4VL35N%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDQiGw%2ByZI8RIfl%2BC%2FLwA7K%2F2mnmC0tPkhFhTKaMcPzIAiAuZKv0jVyhY6LiDcCv3XFyTToy%2B47K2%2FOWJHASWSlDjyr%2FAwglEAAaDDYzNzQyMzE4MzgwNSIME%2Fn%2Bpiz8OYhOmtRYKtwDj9RF%2FRwjw%2FY8eqfFRJPB%2BGgJ25vQqEXD2dTMRQ1oIJEYemntDdtL6j8QM3ejQvz7SjKb1NtQRpJ7gduz4BopeRNj0HHtuYxBUkdiD%2BbQKpLZ9YStYldjXet3sICkW37fQfpIwaq58erSjfSIsMtQwct4EYHmLKE8oyjU6wOSLr%2BQBZ%2BRR5dLJwJa4Hcm0r5H5YsIfGtMdY%2FCBv7gEiMn59UC4CPfo6SxufufqW%2BRwXGeoucoqmoaq1Uqjq8nUrmfiyno7fxdxuqTOeQIuBgRmJG2S51dbvVOYUm03AZlQVlWxQ1VF4wG55oRpOd3xZtvzuxK%2Fiqs24C7w7E4J5Mmn7SYa8cKnT5JQAD9c%2Bv1HnG2%2BTIgW%2BCUkE1KMDUR7iGmc%2FsarVwPxAumfJewd%2Ff3VDNuRvYjBeGM0X%2BasRx1P81FDaTexuszaSKic2yOBz5CZGyVxXpD7B8av4UiuKq%2F3gCO9BbqIy0i%2BCC0%2B7QIkczi2vU9%2B2se1Sccd4FMo9fI8w%2F%2Ftuy3vJMteW2rdWOD2vI6yk8pknv3FiGUQg88a7Om5YbUeLJAU8omj9Ab%2BMs9Wbwt%2FJC%2Fg%2BDQV57V%2FbgGntM0XgBWq7g6dsVsJ3avn%2F1a8je9AtiixmzcUrIwtvz3xgY6pgFYjDwJnElGtf0Dy0ix9s96Kwapta1BprPJXLypZR%2BqjD3G%2Fob05dPF7F9i27F8TEeZQcVmUzXUONYhWVDXTXmJShmQH7i2m8SPTv9ddyx9SSfof6TFpvrv9%2BMBfrrLheGud4JGcrvA6xmGmha6q%2F4UC3sRLyQXpoiTIGheh5okuDbSDTp8DkjI2xeYWZXX%2FqX0f41vB%2Blh%2Fsn831nOf1%2B%2BnWw05vgz&X-Amz-Signature=759ff36ba5a3978fe66eaa2eb71cffe11e328d16132ba9ad8596d3a142eaf4a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
