---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662T2DXC6G%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7adOUJvnX2vo0RI62eIsMOxaBJtDOheADcYyAaqofgAIgcFJb3hpVD%2FKTqLFMtATfYfmlOcK64XxAob4kE15yp8MqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJrIGe3%2BaLWz0X8UUircA9bR8DokJEBVLNOZD3s09ooNnDjE%2BUfvGQFG1a6uwkvecPrre5GzMXNLA846567ZfuKe0p8nOWmW7S0s5c1wLT2Jwd4p3C4KpmPGk9z%2Bo%2FUo4dsCGMyWQIdtidT82sXmmOAGpiS%2F3aXIoXgfym7uFuKPcl0Ye%2BdlR1B8%2BIJMoljkrOVKoqb7YlphLRCfeKBfcY%2FghoIg%2B83HZcslkHnj5RImmZ4vrQ97h%2B0J5iQ2ao%2Fm4Pnlt%2FUpkyzKHIp09CSJFdUW0F8GcOFeieWszLN97OQYYtjRkqIkccvo44lHt1PecA%2FkMhe3U9%2BMBqoQb8bJ8BurlzyAmW6UPfhgG2%2BSjWlgZDjh5rkgwObVt62ko6tRJxIc5w11BRCxgm7PZXGkW10kjR54PXT7Q1ANCzbQGdsqWCHHQEqOaE2B7Zc0M3TpZnbw9r1KX6c%2B4DpQb%2BnzT4BcfVsy6B7vyJun0fjBYY4JEJ1ZHRnPUJNLPbMPuWHTfHJ9o8ehAiPPcgRDjE9ZGQxDS4I3Bu7usN6IkkIN4RiKgSf3RyNzQBjn3PoFSSgy6INmvZeck9CJEnM%2Fhy2XbGz2QBqDxoy5Udgi7cKB9GS1Abh8KsulPVXO8eXnNE6bVfwviTrcQjpBSV7TMK%2Bi78gGOqUBFBGge%2B9feeBrx6JAEgpkwVXdEfpLQ7bc8XARIA8p%2FvQlOu36%2B0njyrOHa6mIQcqJWJ%2BbznMDmMPVNK9B8fThQsmD%2FEVtUH5zdMsvJOxYNZ79eg9bJjlfcQztbL49pESSEHx%2F3AvvAxtV9gagqQHBfo4wl%2B0jSfaNezIFZo89L%2B9cb2rOodZJ3xvpAWaN4cuURkBIvCTTfgD5KQ%2BsiJaXctlYD08e&X-Amz-Signature=bbe68fbc1360c866ccddc1a347614ee3a9c523aebcd17234a182c101fc99f5e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
