---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667FJWHFPJ%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA1AUIeHK9oeLVpJFgKVHgeXbpGbqYOMmdhMg9y5FviFAiB%2FnUowcuue1ctK5DGDYpuuoukjy0QtzEPmXhKWRyVAeCr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMcV4dPrwz8tl%2F%2BGLgKtwDJZ5A0DVwwW4RycvCHAKxiIir%2Bhm9uTx6a6GPNnzaa7qXELmcf%2FulEiE8fqc8fFzmfokT%2F93xqZhqag77XyDm4B3iydxk7RZTrza%2FKaEy23tA2ZN5zqVAdlatOtxPFY%2FZzFck53OFebXRRdXlA3hg%2FOSsKhmGsjn81qejK2viunN1fFd2eMXHq6DOVnoH77DvsMmfOHLBLT%2BRsb6jjHFINSNw0Nu3RFIQTGf3l9r0b4ll6chhBFvW5y9Ai0imszoic%2FjWEHnxr3L2kIdudbHkphZI1HCwwQWWplzHAn7tZrn34r9MKCTt818%2B%2B8E8FHXuW0gERAGbZITr02TQcfc6yxZCPuQ4QVJflQeItWxjBpHEbRfWnhUHxnYERCefG9caR8g%2FmO7i0o7LmEm103CcsBTCKVok0wswPKNRSEbB7jDbhpeQ3O3mxE3n%2FLIYvC5XEv4YzOIpInBI5pNUElkEO8xcRPJXYXaE%2FK5%2FD2RD9eHwlW%2BQosgJVQ1jM5Of0DAQ3t9ZdvSh1jbIGQzwilqjE6w5pA07Y8nPh6beYolAh4rxBkp1mV1qHpwub3MH%2BDZaR3H8CttlkJ7S1YsYdKa6VeEbA0iRuGcUEA8wrZL%2BvKHKmvSQ6m0rISbZaysw9eCDxwY6pgEJPDEO6OE3aYtAwipyA7qTby0o8pPzDUajyDeeZaQy6%2FyIcURjbhc7xhgg34Xr8nV4yyVMsbNqg8MRVSZEdb96eQRQ30z%2FvFVfmKEAqYwg2f6CqSiwyWXfoD59bw%2BckGuMyTCE8tZjy8LVprCz3Sd1PaNUofPrcD2KDjwpEpCMLEHvW6a7nqFVyYsZPtlNxXsdSf7JM0L%2Fm5gKhmhuVKbINQ6i3qWX&X-Amz-Signature=8c13d65ef3ba3f6d343aeeb526947a15d443b1208a6396ddba768ad6cd2c9494&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
