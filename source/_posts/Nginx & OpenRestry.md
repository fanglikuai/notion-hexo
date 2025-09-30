---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWFPDVGB%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCID7BJ%2BsQtV56q6tJ%2BepmU3gMN1iYUBHSc9tG3V5Hakf2AiEArwawzpKB0U272Ex9ItHiW9H9VgOdWWY3BYRvPuYUsu4qiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEHUlagYYm08YrOGryrcA7YKbVvu74DLkrFxwboKQ9LLcbCNNACmnwqbmV2WUy4g0ybp%2FMyYdE6%2BtWiPRI%2FiNqgSyRzgUgPC6YSnkII9ezR3vVBNe0Pze4%2BEdeVzNQzAc3pBeh7WHgBMDxveqCFT8SrRIZBwUGdF0LPQx99SZK537OBSEZ48FNvPVEkDhCFlHoEItSw1QCxl8zao20uFOE%2BHdZvBAspTyxE%2FSR963N%2BnXchNQ7%2BKjUZbUYL%2BYE4UFnU7YSjKfJchpTD4y8%2BeMpf4DuHF%2FR96QDzuvPuyM%2Bz%2BkfHYvBH5FHMzQk%2FJf4pkYEw2ly8gNtt481ZNgmzCiQjq7Bh6gOzSPFNKqIyC53hAmItQvWsq6BlG7C3p1LLCbDK2ghgvajGtqt%2FbdKeML8WLwVfeYXLkzzir5EjH2psfFWXKcWY5LiXQu%2FpoRhd%2BGA331ufzpelGaGpimwKZbd6RZXJgCN2B7r8wYdRrBiLeTLZXxXdfeWsO3g%2Fz7sZWLWN5YnQNDtfJ8bHUuGfbMvO42NJnljpE9LR8GMrOrrcTCzC%2FjPFduYxkjOvFjVN6YoZ9wbZ3Lm5BLpID8ycOW4c8lECFOnel8KRHKv4FsEMMSTSD5xfbIHwBrtrDvpWDxV%2Bck3clI7qzcUK7MIHm7cYGOqUBxKdKcKF%2BiJC8%2Bz9TGEvURCB7Bs2UUS73KHSeRaEaDU4CKGRTEUSM%2BZROfESSHSjUZFwJbP1so4SV2AHPY9sD2RfoXs6CCiYjqITP%2Fo61tJgBhKyiEafGyT5qNVGYdFnL5zHegiPIJAArUkSmN%2B6OGYFiCfkEGVGGmvnlLvEYRqwBIIhBJ%2BgLgvRo%2B5MZQkZXy5IoTb0aLsTWbJkN3hC0Q42FpVzC&X-Amz-Signature=a6256b2452388bdb02d564274f504055f451b4b80d79214162c432434a3360df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
