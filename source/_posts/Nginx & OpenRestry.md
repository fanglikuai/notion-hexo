---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VOLOQYX%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T050047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCwA9D2xQn4Bbktly1eUcQqgnGwvW%2BBnqu2VWzfHvhARgIhAJd8WQ1Oon%2F4uA7tAXRRspw1kDeu41CeLnWaafPJf5cCKogECIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxBfxRxV6BdLFjJjuoq3APG9YLhirfWbcWlYoALI%2FjhCOz%2FdYOe2feHS4Pn2v91B9EDNstkeUgE0JpC99sgWWz6CsplXsvBH%2FC7zEew%2FFNAtu2OGICb4l%2F5HgUhC%2FgcjnujAuXmzifbW3QBdHGl0XvHb5C%2Fk%2B40GYa1bhyq2NA60voK92UIHXsIrHWHW3DtktMRZJI9mKxjrrYEDV%2F8PXkemC503qtU%2B9jecPPgD9vC%2B3qru3vdGVYmSuIOvR4VxCpX%2F3JGstBQyqKBrTI8Qc0mJijJ6PceglnLmi6tTwBgM0wpwTslKQA6uSo8Cp2oH%2FsLsSWmbqNu4%2Bar0TBmm3RsAZaXaVFee9lUQhtpGLbt0U0LvKoeLNo0qtGm4M6ApjYsmadzwPvVpGbv%2FIDw%2B3jPF5dCOGJWR8eeozRTVtF73RrnUdYcmoCA%2FqwkyA4BJ7ToPQKdUt7rmEksxA%2Fox8u2UPP4nje7zU408EzQa4CAHiDwGeitqTE1fJfio14zwk%2B6UQa3%2FCDMbgBhsTfY18oY5yBgiY3PMmiL4QFswnhPZZ7Dl9k%2FKmfF6%2BF1zLKMUf8Lt1rASUYNUoJur61FpEFM44swBVlV%2Bxs8Cz6Ilzw1YT7hq6ABE%2B4wKK9MeMvR8uETc%2FAj4iVhRCCCMDDSoKvIBjqkAQeHOKDsN09alF%2FHAFuZYLN8uWRNHzFxm0nqmPKH47ePhJZVBn37nsjzev1C8ypYiEgaRGvzw3V55RoztR%2FZ0G%2B8hw8bJe8MZwXu%2F%2FwZqBL%2FP06C2%2BFJ6gf3W%2BOCQtEdIVQvf1LvI%2FAVMXQ9urLk94mL6ZhX0wcYCGTaG0vp57tNln1Lh1e%2Fb4VQ%2B%2FaIrHIa5UuPufOltKwWkviasdLGov0aU4bV&X-Amz-Signature=16da192c1b9830d2fe2be915d3f103eb034fe06388afd3615f982c6c63ded8c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
