---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5EHGZP7%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEhnuyX5I3VvXEPj6MMs2JMrsmeBEvI5H74hGgLdZmSKAiEA5BXoS3rahoGckWaEbFiSDWqtgAcHJKFuzdtJ5KwxyrcqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDffmjDKqO6PnPuBYSrcAxnlpAwPevDsHEmuVjpXmcJhqWiqB6zLDmNQ%2BOsPhndDf5v4x1ucVeSzAcYLXxMxvAXyMB8t0Z2tED8ahFzQYkRjx4T%2BTWRovhOCy0jS%2BMKlrUKKcI640LtJEhsAEILa%2F6IJbCN1LkCo8CEaKWQVu6eD0B8fbkEuRDs8vxWBGUxPXmfN97gTApQ%2FrLyd8KeKScpOE9XnjJysG8PoOHQt0SwpWMqlXeW0wZzyTdnKwcZ%2B2u2cILedQ1ecGpdVgbxx4JmvI8MS4LFluRzh%2FJXedC37KGCYXRYqFLiXzmXizOqxUklN5pu6bmFyYTIvqMkWtjsnif4n8OtPb5ECzkeydg6c6i5zbMzktTvRL6%2FqyrASpbYuqGfBnS1JEMHN7q%2Flb58wDgoirzO%2BGAN68uqmWFcqOIygABBFiihLEGWzbYNWiNI1disLCLLWsjE2WXWtLjGtVJ%2F5ieP6SsQlrbp5C%2BhtoRtsvhZQwtTXnoiyqzxCjrXnm%2FCkviKtWHA0F4mjDjteJKXsFvOYH6Vz5E14Md2rSQCm%2F3XnpvScE6ZnOE3tZ7XT2Dwv0XDeY9oMz2vx08RPIo4KVlN9IGZVw5EgBUgjWSbcFjYmhXQsdawX3I0lLV0RoZ%2BIDE0aK6kdMP%2Bd2MYGOqUBg%2BO3E5P9csDiLLjP3O33QHycWkPLaysVMjRj%2BSJjFME3o4V%2BRt5uEjE5YLCroD9%2BNkqgnrWBW0kuzSqqHJBRZe22hzu%2Fv8Q0MXxTQyu%2F%2Fth7G20vIVb7TkYGoSjI15D%2FUL7X1WYYZualAmca6xkNAJJxIRB3pn64rABVi9i8D6Ug4IFhl7BGmeNpg2YAyDFdrqDw4gGaBV5PwqOakJtzbAJb6NXv&X-Amz-Signature=2029359a7fe2ebe6b2eeaaba0a7adb4310ba8c9961bb9f1d67aebb192f7607a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
