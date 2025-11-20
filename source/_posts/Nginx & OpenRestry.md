---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPMRYXDL%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCsbDj1Si5uiM9XDe0klFrH3c5q6XLLgh0%2BIhNeYnNRPAIgLNLnzc0sL4tUASPYauI3OM6%2FpwC9PfQq1mRFUUa%2F3n4qiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM3VGnHB4px8jRe7ryrcAxWCK58Q13rOl5f0j4V1pELGJuZgDfA0SaBxEhjWMsaZZ1fpW%2F8j7ClOpDoFqeG3FhCrxx0A%2FkzY%2FhOifyuqFRH2MxSVl32jVm00L1M%2FJoi52zXrGyB%2BIMJU3kcPepCmrlraAa13vIGHTyl%2FPS%2BJ%2FCSwXwwkADEX6ZcIIl%2BGnusrjrTr%2B%2FlowR0w3X5DWonY5HSEcfN6oc4q8AA7uIdcXjyRtG8%2FkHIffERIdD3ZnLnFvJkumiruW7%2F78RPV23A69b%2BUrbHQIUiCJuLK35aVXoSCa2OD9miO%2B31S5ww65NTC%2BQgd8u%2FseAKa2wiCRku%2BybQntR207vJu4MnJKxlUuFP6uOYW%2FyuHMVMmKEJpoogvGleXxWD6zlmRsLv8uWJv7e3xuualaPArStm1j1rQp6TLqbzltX7Ac0kgtMY%2FqbbiZUrJgTKrXy%2FHyUY7fWQctQ329rrWGyBcWGWaMg5JMM3tcrUigI7eobAi7UARL7NBT8bSIFMQxA8QMxP4fiA2QUr2vQkCo7JL0z1uHQr1r%2BmzikWgjLmxyr3QqstUJ%2FoaDXzhuDtpqXimQlTdCYaAeVvP0i8K%2FCqsRTUoJ5hq0CdaujWztZh5MJ0Hn9%2Bq7ZxBXfkWfoPN99AVS2rnMPLR%2B8gGOqUBBLnHVNC7xVOJn66URvZujQJORI5R8Havmf%2BCxHDG4C%2FfUtRQ3NxkwEDpm0Fj%2BVj57SNLYvBozN3ll9b4LdFW3hnGVDocXb3EUogQuoS7RsA9L8HBWug%2BOUxDhhY8zJFjVsOhbD0wy59B1hdHgRD3dStVG2hdoS9BrTMxlOPqaCM70KUhn%2F0yk5oxR94G1wF8XkP6d8%2BUwTrLBJ1bRc1TFIt2qyrq&X-Amz-Signature=3962e41f88e9e301f8bd3bad8a45782943dd58608a9098cb655ea3709a97a33a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
