---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTXQIXOW%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIEljmx8e%2BCRu%2FasRqfasYyyq7Tu5HYMWo6WHVIz9HGU9AiEA%2Bd6cnsDQBpnMKzRk4YqGrn8Ap3BazSd%2BnKNF69pkys8qiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH7tj3adw1SofiChZyrcA2iYmxuguq%2BrIdohUWD5upAuUdXy7rQj7oXkU8RHVyYoTVf0%2FRGXMr7tPXGUfQJAabObFpMBEToF3jYXNAQ89O8Y0Tpt1YFQ3xhg4mit6viPjzfgYhjsTTxIa3rIY7Yj3HT8pQvbWWWF0we4poGcF5mdLduZxmH4acznWOnXXB2v3X2yBniV3bm3vOMJrqeKGkCYItIIFhbnjsevYP%2FNylUiuIqwk0mrOm9q2gpKE7Ig%2BdKnzKqI227%2FKEGe%2BIvOVWdZe0YPesLsZpFay6CPF%2FlrXpL87uIwggpj5cAjvbwmIRzURZF4HMZCg2w7cwxpKOJ7guODeegK0Icslk9cPAFpa9s8CBwTFqUMDp3%2FYUCa4VaFqZHtyg7Slb7g%2BosR%2BA9fGsDXJmYtNThdPJPs4Hsu%2BidMuYAtvOFwxshCCWZWJ6yYQShJfRN7RkkdNBTISb5mNk%2F2gYoaDuEUoQx46bOdvyrbdqGcqBl65bd%2B2iisoUoztRvluqLE6FtWLovscN3I%2BQ%2F1JdTUXHZ6iS84P0v6RUN1edAqKhTdQhEOrgY1EKo78IuEb%2BJAa%2BqVXmduQXkbBCbaQN479cxyCIftIrUbRh5OoV3RyqTOboC7v4W5sQ1qdRRTEvcJ13vpMMfuv8gGOqUBvlXytjhIn%2FgK0OGxNtF2WBv2BhC02hZjwdzDmivIIGBaccHhZZQjphHJmUWwgFmwbcKEeQbnyKx8N%2Ba%2FKkAjzuEnCB6K1kh92haGqps5NqCZgG3O8F6X3k%2FRa0w%2FwhvQxsCEWsX6ZWjRNmnrH%2BMdvcklXJTWlcHQIEYO7m7vhgIvhO%2BZiO6P%2B3s6QSBTs%2BA5XQl83ekYmdMvuS1rxzWv3L2LDmMi&X-Amz-Signature=97c45fefebdf7c5cb71ede4ede99fcb98394bef04669e0590a0aa1ee96b568bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
