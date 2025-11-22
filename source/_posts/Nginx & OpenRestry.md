---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XU7R5ZAU%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHJqH1Up1qYDgFjdxJ%2BU%2BLQNvbMTz3yRyTuSaoQHPYy7AiEAvlslvcuydaRhipj%2BPuKHSmpse4v0w3KC1%2F7QX2QLsMIq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDPH%2BaC1xdGFJvAnk7SrcA3uYcftsIW%2B%2BintdC9KvSQ1E%2Bj3hNBlOP%2FUHeIgQvTYmeKTMkjFJ5RwfAXGi79Ve87w4KVCzZBxyEjqETLlTQAur%2FJ%2BAid3NR%2Bp9V%2BAnHQnnUnN36Kt2mbPJzxpN1rpGTjibq%2B7JsZiPJyt4ErCm%2BbeXVl7Gvwqa7f1O%2BpLs6AkfVrNKeA9uuqTmRALCuycpB08uevQGUDE4y3hhNwCfj6uWxHckZvVzvS%2BDPBtVbG5gGl89%2FISdpBGuldOHnDfSG9EXg7FAPepaPb830I9f70ZIjif7G9kzsRSOACSVOsgPolGFNprLI4rcFWbtvz94QTBxyJpNHYoHHwn78PMrJj4tcJlCbhhXuhtwHR5d6WlYVaTcwPIAxKqiV1jsQilJhkVXwjf1kW05DzDAyDQ85t9D2ZBzGTBtyEVulz3lp%2FUx5sYIbV2jyN5LiIZv1Ke06aY2Wo%2F1YrcZBecIX9r4TcBXxDKVEKM8lvCVN7MILw53ND8oa1kRpufz81qeap2lNVeStvYEETJOpHDpp4JLC7brJflpfV%2BSzl5mxiJvdUImroxWo%2BKZNBNyt4iV7V7uYHf1MLA8gH22xTmNDUlsjTvtVGFuZczU8GzqwIfszQGL1h4uRKdgo2%2B9GzumMNPKhMkGOqUBsmw%2FtsXFhE198NZVbwvN7rrEMsDfwDBVvyfARUMKeVkJcpw9TKGJzCFxLFunkaaf7M9TqxLpzE9QiA%2BEBADY4SSn5lXx%2FMajmgmlP61cKHwGKYJYdfsk3T3YmA0XBmsgrCzVNwuI0vJHN8PC2bOp1JV3Nj%2BNoxhZxXcjFiNeiwNPYSFjgJUgsF47l%2BU5jhVH5zv8cd%2FvZESEu8EuD9vZGn%2BuI8xU&X-Amz-Signature=c93ecdf8f6618e9f29a10eee42e3590a61c30d90fc5775016e5a0445d961541f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
