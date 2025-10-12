---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VF32OY3B%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJIMEYCIQD66ChR81H1HCpFATNveJQUXIwuvOM0Q0imYfgCmAMk9wIhANXbFKM8tHTA4wTTjWWM%2BNGmh50zAfvEMV64nWgrMxMfKv8DCCUQABoMNjM3NDIzMTgzODA1IgwNe32jIFBL0wLmZVkq3AO639MWJfPfVbqw7WBB%2BxRozJuXcq6h%2F0wynlITeU78HhAUG4H02haTCPRNrNWU7Rl%2FksMsIKNjjAuD2bsVW5coy6zhW38ID7ljhRGLkQl%2FzJQy4sRInU0Yz7gvMDHRD09U94MHtBIfwcFexwzeo59bgElnK%2B6G2wOzDP5%2BRqQbqJWveULKuk51ptHOjoFHv%2FR5LLkhXoUcKgE%2BDNQjJYbSc44ikXdeH0z%2BSkBccOokffSxvMzXnhugU8YvpVDWFEksG4TJxCp3khdMjxNSmUQZi0yLk1Fg4V7meANfVIIo%2BbLOqUMzp9Y20dq3ETgsZDXnjvdWeQZTS4a7fDI0lvrB1%2BuHmFSbhaKUr65%2BwFIbzxpJV%2FESKCUMh7CDKzI65Fv%2FVjzyX14MHYtSS2zKnsx5pRX5kBJHBZcYueRM6ecSEwlOAbbMD%2B5i5L0DunKzLV9o0It2avBNPPs3qxfi%2BH7gxFKkzXsGOZ6VRRzQ7CsNPFdajDgLhuApCsa1tHPqEQKVgCy3eECxLWPU3bDgSiAmVareaMutXyXS2yPIJLk7R31olahkdPFS0Fk0JBbNjkG%2FVcVcyL5eCi2vzrQBUlt79q%2Fvw67g4B93DUJmQK6gLtTpEZAlPsllQ21JZTDOy6zHBjqkAcgJH2AP4ZGx2l5X2OQZBsw2vy46x3DA6b5fsWP1%2FnvAx4Od75b2NK2jdxszK7HRY65vQsJg5%2ByBneJv3EwDCJ2yi4HRIc9erduQuXSwUguFY8MIDcUTMEpf1yhSt6rokIWeAIMJeXr5ax2XYwyXO70O99oR44ylZP5QgQx8HlG90NqeMPao4I%2Bo7kgg3LkK1vLlFDJQbK9KqGkQHOjIMlIoxlYF&X-Amz-Signature=8d5813a45b8a7ebcc78b7a011efbe31258e1bc75f76fd72ad5236b5c249e4356&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
