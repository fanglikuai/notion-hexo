---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMB2KVPI%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbKmNBSzT5v%2BRdaOPVe3M9IAhW6GHEID0ve8ZCtr%2BjQAiEAnVWR1GgaiLLOIr17YAL2CdhkDHf0jlRurZkSmd6Pg%2Foq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDB%2FWr5hoMeKkfiYY9SrcAxUv8FOC9A1arl699K%2BFRlmoqjOYvbjenW1bCRGM7pudf3cfNouvxgiH2u7ExyHZFCxm%2BPyeXjMxiKq56hddog2lL8URlGorWrH4hGOtU7OAYXYIXSAb6np6f7phI%2FrW0KZ4FhDUn07xcxtkPL7YwEJHVArPT5ilcmwBDYj6Xw34BiNf%2BwsLeQDxMP9HMAkLQpfM8lLPyluJNJuNjByweCcNEWVF4fIjnXK6EzMhZwJTjUeIk6z1BMz8sjpKzCQYaCKwwnixRlhCY7No6Lt98%2Frk86IXOxGNbNofFVuNybJnejUChTZgsHUPQtfEr76PvHSWs%2FpKFSLPgrop68%2FdUpzFd4jydC%2F1oEoX4RLQVloHmpXgy3yJ4SjJNxTfqr0PDHCgJyWhF1tRWMsvwpxg9awtYCDd6htorA2K1dQYNPl42ohEOMPLYDqYcgcS2tLcbllxiTwXgeLjSW0j10D8ysmbnf3JgDCXhzTLTmPfEWsUMdDheXAAg%2B24JgW3sojXJ8NNY4HPZsq8OPVNuXYr3WdaW9PIhV5vebr6Jtzl%2BSXBApc2Tb46DHyn40Bi99rEBWBm%2FrZaMaU0E%2Bk70nEqpXjiX3Lt%2FV7tdBi7dtAGVJAsy68ecu8nKwWshp8xMNmmmMkGOqUBhymAmlaKbGbUJx1E1zhGMakDGorvAOjImZb3f7Hu9bGbfBkdYCSo%2FF0PnuoH%2B%2BOkBmAnw8OprZomUYzZxSY8fTT2nw3HwLIgEadPVCJ3P1FhKUkaPj5%2BG8%2FqlIVPI4jD%2FvwEPD33OCSkPf7c5jaWQNkZJgg7twQU1yIg9n7anWTZyG%2FF%2FBXxwxc5xsjN0UstaDlA07HEtUcOiqJTPFZiaIjPrP71&X-Amz-Signature=28cbe8291c74567ccd915c7b26fe506611c4b1c819b2327272ef9297870d875c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
