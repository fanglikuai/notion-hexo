---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS5FHIFB%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T120042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCrdgmePFiEmDuJgmAFp1fACvk2TylrH6T91DDZlTDEygIhAPH01yiFg5HyXTe93LBPHNX3%2FHkvd1dqN29M8QFwoX8rKv8DCHQQABoMNjM3NDIzMTgzODA1IgyOloRNByJI754rSeIq3ANc0d5szjtUHI0mfzFPxetL%2BR9dmldzwU%2FhvjbViNr7IJotZZHT6iYeVVy5HOMOA9u0PjuxdnJlmKrUtKmGinn5o7zsB2Neas%2F2G5DsQHkfjd0T6JdkA8FIIUOnxSQMHYs3o9GKJl1ebIHiCwZ2Rp%2BZB1w432WzA3RGnvRa6YXPZWo2AzYnzA%2FpyapGGbDkLI6mKQ7eQ8cPu%2BL2ZRlUWbL4jOZGnDrxeyzUnsZQowS3Kelz5Ixq2hbo%2BIlz%2BNTBurtKoY6uBl6uQDUtchiZS3W0eXuU%2FrrTzrXpaHU17SpRopuK2pTb%2BI2gR96261yBigWgufrMHwrMS6dWEJuv1bLMtlhDfrYllnciNkWINRoKY6IAodLWCfTV%2BDTtY7fcCIsWyuSXq7G6nM35fC4GlnnlazJUsB8HK6AdR4CT%2Bo1HtEJeji0%2BoDfuXb6ZrUDd0uyhM1GG5qw%2F9%2Bgq%2BApr27E6JOUJDrHeEMn44bSINLcRRGPMyDX3ZzW%2FToV%2BV%2FW4JOnKFTHCjrpLZU8LpEsfbPkiTYacxFavB8kl5WMeKH47%2BE5MEwyFv2KAFX5FdOKUlXX7OdGttQDOv5Wt6%2FTKAlbT7XvwQBGDZII9irDweM1tFUfKq6SRmPGCFG1BiTCpn4nHBjqkAUb3CuEcaS%2F2fqvYMH3zoBjzGmn4wpYdVU2C9Y2xycMJV2vO6dz3iDOuIyxq3aHHk7HwK2BROvtsqJXeEeWO7Vkv181l5lkuHZwh4JQoqMaqUehjsN7dVQXBWQChjJyqyen4l16GicKuOtcrz8uPf3UeMsRJJfZ9lQP4EPdyse6vunofzdFQBYGielobSaRJKwqVRR4ROjTYs%2BotDS1%2BnNMifETR&X-Amz-Signature=503472156b6824d65cd8e3a85eb5aa04f644aa80cdc0d636170991d3ded2b496&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
