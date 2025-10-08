---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WSBZ6E2%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJHMEUCIEny1jsBG87kOYSO3GSyvY1ce1CiS94IWwMwy2u7pMyDAiEA4jDAMOrT6zcKCz3h5TdAiReBD8ZSAS4qIGPgVLsvk4EqiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOm8PqUZnc7uXMQYbSrcAw3SmhzpdcO4rNH1wv%2Fz0x1yYiJ%2FRm1x9SyToQplO4rHq8spAxh%2FGyTcrfrPZPZRa9d9qYKkC8e8TuXVmZeThM7lZhwSg%2B9YN3HVo2JUg16wGu%2Fv1K%2F70hIrm6XAfDBTB0PfxWexRT1YcsNTdfpK5tB9IFs%2B82xSYTbGnbj3ILVZ4%2FyASchQ08q0UOLRErKZTYps6TvfI5YZXAU2GUlX833vdck5A9xdvGJ4KOIMga8JW31NB7WcJIFnMj%2FujRtQVaaX6QWmxnhr1jwXnDqjp%2BD4R6m4vhbClW%2F5qv318aFjgytai5q9vKBbTP7snjEouYNPPQEw8sbz9hD1HIuqcKA1t88CccTwleChirNrpp0W30pti2yuVGtr43h8SjbdQg2fwe0Jj453jeYXkNt8x4bxc%2FijUGAEOelzbKTns5BDVC9CfobU5Mu%2FhXvzWNT7u6wXYeBEImHRlUfTe0NuAeNWthmEVsLtSXjsc98%2BqHeu1OxTybBQjktuf7H%2BeHLdEzsy7OBGQCTt5oby4YNcD9KPAc2yQZn5DZC2RSbLlNSfQE4wB9niu8gx7%2BnWHwE6QNF7fkNe7bqO0N6nWI5sfwBF4%2F4zZqOkTJ2jTtBzAx15MnIYw2phncwtvozGMKPOl8cGOqUB3Hp%2BXTmv2ux1%2FJHs1YDdSEBI1f%2FHr8ti9RZf%2FcEIAeGnAE119l1Ar1Irku2Ww2NRPoHR8HWc6KCNErXaEVJD9cQ%2BvxxEFiZrg5mpCODSwa9IS8wjo0D37tc9iLdkLEnGI3K5Dz9IMFp%2FbTM9LHiMQ6HM9CP%2FmqMlkwHSCU%2B%2FTBSq9WmUybmWRP5uuH2Zbfpj1w4v6Z0YcE73QGsnDU26OghUMRJf&X-Amz-Signature=5fedd3e41ce96f880ff35cf01214da424e2e68d49d6431889d6f1e7cbf20a154&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
