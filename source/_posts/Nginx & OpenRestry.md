---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCQESK66%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQDtKNJdS%2FEkQg6PqUDjnoeN46lLHTv0dDYBeOEwCr%2FMQQIgLQtuR1wVc1R8ZM2FNfqkzbkIy306q89TSptau%2Fm6o70qiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD1Tr47Z6sVrj%2F5CxSrcA%2BJqPeo49hZFow6kTSwgoqQuv%2BrK8l3T9uPW7K4PGAUwth81uYfekuGGKsdG42AwY%2FPVu3jWHx%2BjaocHku%2BYYi8YnArWtBWMWzMVt5fwvIAF7tkqg0vfmyK6Ga%2BOOqCzzfLGP7AIY3XueKK%2FGu%2BFFbjiD6ArV5PCUs2Bi8UWmVlEzWiulPRdmVtO7H35Lz0jAKbj9SMmvPdoxb9O%2B1BJda3ZA1I9T7IGECqhW%2ByoHPoozlq1TKVvVwLHiUz1GRiAJHSsXZL%2FJndZdAQ6BlFbMcZJL7%2FspCrIN%2BbtNdp8sz7Ee3Ikl7ZmuQhxa7pXEtOpoissDenXUNrk44DB6TfjQxcPfn0lRPbmiz7yLkXplRWNdiwoWbsTohN1wBgySj7en6Y%2BSwBtp45jwI2ryXU0zEP4Wc5mJhiWuOGJvwpkfTFxI2aTO4O%2FHeCDLuYQVkOv44iMeeSKtsaieHc9F13dHWUNNKE6gmBxpFGUlQph1kFpCnmoPVJUzyH5xz3jDbuNsy2XBt6iZGqa1S2f%2FUwZSkDOJcuAbiyYEmMv53CTulqITPVSbFh0cp4sxllXDAiRXOixEof8MQZh%2BxaDIEbqWjE9e1t1vszBG4uMCBJGxOJaozCCrDZDN%2FG1%2Fz7%2FMJXuv8gGOqUBg70e%2F6EF0%2FEQ%2Bu3wKeHV09wGi8kGtYuov%2Fs2eBkOu8HuPnoFqKvqBf7Y%2Bzi3r%2FHaCZ%2BVCz8vA0mTVAubhHgtkELU1X%2FTF3NwaFyNTapoav9uqqVbN8SvG%2FM76hh3ZDYtM%2Fd3kA%2FLSpiAZhUF5ljIijb%2FgBAKLV1RAC%2FQG%2F%2FEgtq0TawFbmw%2B3r5KXJ0CnpvRpI0OE0eNuJt%2B0VLNJlLDiAUkdG9p&X-Amz-Signature=43dd2a00d5cef49f6533ea9b4f5c25ad779987380d8f7ee158c9f1465536768d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
