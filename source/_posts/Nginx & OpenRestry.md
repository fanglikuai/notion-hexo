---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622XU7OF6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDoGS22BoBFajOXthKI7VzKs3YXKnhk7UMgkKHSTfxc5gIhAPcaChQ1f5uIdT2yMg1dMFCBPwP1hMhO1gSB3EWrivFbKv8DCFEQABoMNjM3NDIzMTgzODA1Igy0kpMDIndqvC6uU%2Fwq3ANSMcw0G0mWYauzql40%2FbHdndEU%2BQd9CQExyV4p6tLnhRgIs9KuhYrWBalkgN9FNSkpLsODcFYRMrjQUd3%2BA34HXYFyQJnBOpc4WKdYadCxVBD0V%2FA3j%2FtDWt4a8vTbD%2FZ90PFICMCNeIYhuJOonUixgEQCpeJzVIHlwMMhjIURdwZGl0GUzj1X4KplO7iJVajWA57%2FAAA%2BYCoo0lJbQTYzbXVDZXs7FDd2RWuK6NZvvnEKwMW9Ew287lH4xlBL9THM9ZTkyoztgdRWehzkn7F%2BB%2B5tNvCJIR%2FofUi2dY%2FzXTdQmsNhmMGAf3szED2j%2B2BvkRl%2BZyemUrIPIVxJ8B54OrokXOqHWmnMN4gfQPnkIevhO6f7NYun2Xs4k5xEZXzxh0topxKvJUyCDzbUF4rLwfHVfaqrYklNRW%2F%2Fg0wwZr98mSOwbb1aoJyQVSNw8qfxGFUq9YFVBqkIE0akGVfgAeZ5iGuuv8chXQAlwy0a1tqHYn2gHV17b2JuX%2BGVAW5FoD1%2BkOueDb%2BTqljrxP8krxfCy2u9WxJO%2FIT5YiSxJNA3caUqpzdLfAt1W1ITIplvGNqxW0lKkT2nLJI9XIUIQsDufHQFnCL%2F20yRtJZLi5JrcXGvJcTY0E68TDCS0IHHBjqkAVV3jlpp7wjbc08wDn76rkBPDYYh1REw1RGnG%2B5GHdDJrsmM6ZKdt3wDNY3AtLopwh0GBw4%2FdWKJAJGXr%2Fv02d14N0ml%2Fbp3SfEAmZGMSSHwYfG1alHn3u7yzzBt6ZjM92UNAH0XyEWlSpoGLsmVzDNZYkplCyp6aHuO3LBEolgJmzJibfijdTl6j5O733mS%2BECuzYIT%2FNCnnqsbtHwvxTJmhM1Q&X-Amz-Signature=67b1cd8db287f0adea7a9f2499eedb234354fe49638becb4dfa57d09ddc0d0fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
