---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JNHQ3GJ%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCxtjXUBswgNGXkHo7XPH7Xkm%2BL6GnLU7fnpN%2BlFhFwMgIgarFcXfwyYfhUGy5Px3Q6u6YonBuBu0T81eBoU%2FO67ecqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLuVaFBv5%2BHaqag4YyrcA%2FIpvjGJXFoCIMMBkK5iDYXoeAH3anShNJ2zfQk8oq%2BGyj1SegVqnoZTV3Y5V%2BNqElOrI0%2FIXq7Xz%2FBcSDcvMAdzs3uzEWZIfsQeolVcXAoQfuSqkYMm7%2FZppTlKI%2FH5MfmkrWptf%2FJxFskso09t7CiuI%2Fa810fOvEeMyWFTlXRmP5uohgSLH2RDD5slqK6jG77jm8gvJv37%2FRUJCL29YWsYs7%2Bue%2FlcU51xb1nuSU6jT9mE0h%2FzwllLsUpVdedjcx7k76aW7MYrQLOCmYpaQ0quUT41JlsH5cHnWPeqh1M0x9oA6i5cAkwW2576yd0BOsC8qZirkBQSmbj8%2FI0hZVXSYvbVSIyu8wbp75aWhqpb64FzN%2B36hmF6UyFC6zCvO%2F7rnun0R5DSXX4W%2F7g9xFh%2BeuXD9Aa3NSwM2EO%2FIB8jGN%2F61nQG%2Fc%2FFqHDXZkIKjl3iBS3xAM3uP68OR4kzpk2Tovkhki0KwjiaodK0u6UXtf0wnMYskUpmnEJKXgzwOwhmPy958RzJ95KASmJPUebwlOMgJuWMIyBW9ZeNHxqa3%2F8OMhmLbkE3XiV5E1ogjLa7dY%2FJfXD4vK4ukEkruGbsRpENCk8cR8LQMdCj4XSIDlkLAtg9gEFb7p9LMKaiockGOqUBgwxvwY0F6fbUWOitRv3E3AdcBlifUoSEu%2FToEPxE2e8KUhMsRbv7oxZya%2FJ7fj3es8C8Ca3fqAJSaDI%2Fdzp4RfaCs8jG64w9g7F4t026Iwuva1oiKSytNOpsVUxZlkrVsuCCqq5dQq9%2FRjzQM3uQnNVpzcD4gky9NPJWqX3%2FzmFX2tkDNt2T9geWioXPFGR3hkKgOcTAUSx7yDO9qWHDCWTzHvdA&X-Amz-Signature=45b985f39ff1e07058e6f0b65a89f960218bea37836a39d25c51c5294a0c496d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
