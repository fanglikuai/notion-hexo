---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VCDJGOU%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAzLArQ15PfjLFVE2cCZU%2BvnxTb3Xfl%2FLoFOATxiA3ewIgBhykf5wrZef7%2BwPQy341fDXAjK4eNdzHCEFaktG6%2Fkoq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDARmG8AmPJE1mHQXFCrcAzSDURgOLJup5JoixQXkw%2FHi58rMu9bgdvbXAOsMxu1ndE5DukGyYL7ggUfCoIixvMyNooLsLJp9HhSl2xA32vrxcgJBE6xQ8cUJ1lwmJ8BEhUw6rAlqS1gHokO%2BPwiN49O%2BBK89y1fL27VxM%2BhasBgBywrFXscjs1KBtGKDh8kX%2Fu%2F1iRV2yGi9%2BQwTQJOYEDK71CNyDDGra4AoCveQafORkxb8bMB5T1PpmpP1v0UBtXvQmQTq3i9JSJmrvnTqmZqH7V0cQDkSFi9Ayg%2F%2FTwqeTMpyq9WAqn6m7nThFUBCFBspXxw%2BUAtFtvwVowklA1oLSyjDcrjrKIiZpVgbe4adqRYq%2BmM%2FNtjWQZ2Upjzxoz%2B8zJxmqImc%2BwtogX6XkdHskf7Wkc9NfAonpFALaMuaxhU%2F4p2iQD9ueN4P3UymHZZsZW4i1Y7Qujm9IyO%2FRZqPULCPVl%2F0BB98hD1Dq%2BsalXu1Rt0fekvAKeZ70uV4s%2Btk%2FMtUGEtjXKZygHjLIyvOO07DqAfSZZGQ9z8c%2FeJIy6UiD%2FtfPpPNNDWk%2BIOZYD7tBtervFrCbsR5OfCETaIXLWQlMJ0SwAzfloJf4Nas%2FYnvGkfTFfNAohdTkmslPx8OGppYqj8OI6T3MKe7n8gGOqUBshIg%2BRnRVzRsZL7CAbAdx3tHT89H5cA3Y1tsWLOd5789f6JM54IcckRLWB9g3D5viFe47nzye7tarA58fVB7Iyv2po7nmzGXjKwBXdyd8sgXkgS4Set0GOe52EQ4%2BNepm5WUBvNg54F27wCPSIvlxqb1lDHVCPjqZNn0l9SJOYFn9PUDUhRo5XChir1Q09FofJuFyu5cesArbWiXOXrwjgB7WA2I&X-Amz-Signature=88e494c20cf94dda33204751d54d5d0a6f38ccac6eb5eb98aa32998c346bc99b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
