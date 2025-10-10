---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RXROGNR%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIBUYjZ8r%2B2Vjo%2FHMPY%2BQTYk7S8aOF3VkPiVdzrMpVdkLAiEA19F2w%2Ftm6SSn6f%2FnOXnvImGZT9GyEPB2OQptLfiZhsUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLJ%2F7AnpgA0TO3PJvircAxMkV6%2B6%2FHkqkG%2BzAUQZ5bpzpoIqBNAgQp1oKwiHubXRwFUzISrcytosi%2BwMFe96I7dNDyw0RSPUchsmApOEMG88LdBSJ2WZxyhR6NdKgPb9HhzIasCjfHfvbeNeSMLBxpWSiZL%2Fjn9Ze1RUcnkXJX3g0bAn%2BmBqXCQfHFVuwdnEeukF7QtzJXjmkAh4iuquz%2BAPzS0JRUKzazp%2FhNxMYwsU1W5vLnYHY1Zre3tVsezQzNRTXWFckC0tyNXHVXWGQoeFchNIgGrSc5IilquRtG94fa%2BZZGkQA9%2Ffg8FTY11B0g1cC0kKb8foreLVoOtb9jTTA0RsSIeI41tViiuiuRvJsywa%2Bb3RFzeOWDNporiMxrlsn7nEpvKp01kqzG6tJNQHCLbMB4wzPc4Q%2F4piJGbz1nFwym8mR%2BOyaucsXagy%2BR39dtJa6FyeNY7i0F3IVlVGFvIPrJE%2BDu2H0XS6L9T31Bv9yB%2BN%2Fx9LFLXSKjAaTn7gE6PEA9lAWB0q%2FepN4VmmGwOHQCTMZ0bu7PXsv3dKcRDLd6cRrw2Lv%2F6tMva06rBg0OYlTjecPfGuGyJXeLkTahTyD9T0X56NA4%2BFZDkW%2FU7FAWwsqMojkRos0%2F%2BcFtxpFc1JJvNnWKpkMNnZpMcGOqUBDqqHxRckJkN%2F%2FrafI7IJnzQxlSTmhgnD1Fi5neYoTwRXG0U58Up9VBupDt0vZdh549bLs%2F0biwd%2FF6VwkBe%2FqShJDkanrVuxYU1iM9e5JFoCArUy3hfFbvR1EFFkqpeCyqaSIKYKzAORS6kxjKXPS5C2jPkxsqVPRqgqiUbY7VFpHZLWDMZjKCwD6mrD6G0GXZA2Fqw9vqBI5QvbnB2MLNa6X%2F7f&X-Amz-Signature=3f0f99973c572db1b82af3ba5eecf9a8d9670df728d650de1cdc2805baea8a0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
