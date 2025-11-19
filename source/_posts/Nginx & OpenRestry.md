---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDUQEJ5I%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIBnwb9e6NFMGkruwcDSln3xrEaX5wxazyGtfY8jP0cauAiB9QA58BHmyGft1pQRam1uRCOC8bIVq4gh9kfwdBnVV3SqIBAjg%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrMa2%2BvqhzGuoN6yDKtwDJCDcgvVGpZPOsk2nwKXmvKYSnuiV7efjUQ1WD0jCzV7PQT2J8WctsnemrwwPTYF9QzSMGw8B6tnYgwKxfN%2BvQotFIj6H5nOBlr%2FYtA%2Fe5SgAN3ZL1szLJK1FFjEGN%2B7hJBRgHVFI8lVdNL6U9x82lixarYQznO%2BpQq4u4wqJNvN9nd%2BEgKNq9eMTwj9sD3KGTRWw404fvXFnuUQ%2Fqw3R2w9bDnxX6BF4eshDCak5AT%2FOxJuonhBEmQ0Q9ofJBoDRoYw5%2BCISYQ0peX0BF%2F4RJ8zTqgonYAw8Dnnd27DD03YL7tYkjitXFcg8hHIxEexZDmbeYox42q%2B7OUqqZ8ALpeoqAzh%2FbXustzmevjTTOSouwKz%2B19AfDHLtdJo9B90EqGrrvjS%2BiHgB%2Bgjqfm7eqvcqST5%2FIfwOqV12gCHovHDyIMRMC59mjSoZBYGSl8wOMBGRcD8zimPrklfjIQ%2FFr0JpHGftirhGLMnHmOm9cgn2Xp5TG4%2BTZ9hXiEvoF6NstpHCMjUOq5hJoMfdBkcQhoCn9%2F3kRsW2v8jpDEbV4vfWv4gWa9NpKSsslrANeaa7As0TO7yQ6xyqLd6Fn2iWBrMJ60C2pdddbScAKP6ayd%2B%2F4gZ8SpIPafy9V84wj733yAY6pgFJlsvy6eKDgxl0r8GySp6GYRKs64ad3fgUhDIxkBkAUssgqWv8b3Zt7M2j5oPieOAGyCdA6O5ZgfnvjDBn8OHWbvitEMVT67mdhqveOoljRtpsozBuyH3u5t4xKLM4FxW4ABOrTJ0GSq%2BYqXaTNGT6YgPWySNpVlmzF20YXakU1s%2BCmCJh405G5mX7onMAdk3yYAaQfprePhanJJTqLBZblKyiCTAS&X-Amz-Signature=02a550247bef3d55103c607eb2f9f7353062fb0958cfa537adc9a6758e9a400d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
