---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GUDNUOG%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T100043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjQiEnXiT9qxqmO3yxK0c%2FSZraOEZrfmsm%2FheGgKxbTwIhAMhsOHU8EQBQnyY8stO9UGo7evf70Bm5zaAcQ6L6kuOaKv8DCHMQABoMNjM3NDIzMTgzODA1IgwcWHBL2E0aqqiSFBUq3ANk9a%2FXE6w0U9ZjwqBPWdqvdnk7X4tV5ACuHEPeHs5nWKaTSRsQBTnUAUHxKJr34nC6w9mnk21iFGnnG4M6MkEsW5SKaEtXqcR2c0FzIg25GMP8K5zYIEmpepLZKhVRjFV0tuUrR3elvC558ul53Py5n7fUrRfrDMYKwgA9TRag4ZqDwqrLXbyilR%2FTm42kklQhrZxJPYjK6H3r8XptpbVNLrSbbXYJH9CNY0d9fLLRnNhtPw5GSKfh8z6zG%2B53ZjqjginuH8Jbr4N9nLVXTWH3XhSuWrw9PMs%2BCHvE4a3AMmC0xQ2RlFktqJcUnH4Z7wUvFpL6k1dqphTHgXkncckgEL6R%2B%2BKqYYIBDUGZRMIF2HiLwQFB6%2BqVfHkJtdF1aVmRLUEekf8ib10jqk2AsldsOW93TeLA%2ByaY17PiM%2BUTnH5OhnRXUDqrCXGQMzaV7m04TJOl0w6ZRc2Q4VG3BjN4Z5Qj02LXKR6xz6oDLSENmm7u%2Bjjcutf9NieEG1EiXMgzlQIBmjSI84FdNqrekrHsMqHiQmHQNbHYaRj1WpMe2krLKu49CwU1B3DXp%2By5nUe8nq2cMhXoi1Vr9pf%2Fhtl0UC4EZdK3bKyV4wzpHkT%2BjZu%2BEw6pA7fDLNyZvDCZoNTGBjqkAU0GKDFpf1Fhp7M4DYTeCKuI%2BT7g%2F36CSqeDHqBJPgMG10cQ5enKelcKuYqoQ%2Fi4iaiFbjADSr962Txh6M0IDv1ZH7pXXuWHOCpb8dhPmYbkq4o%2BPXpd7UWcR1a5pkbRMMq%2FDCGu%2BtJ6utZtmNajpoOt9Mg%2FNjoTadrrFWgh3j5s76XAvFMRDtrFwZ4JGUk2Hu2xd8Kb6bX5RafI7aKTQDuAZIPO&X-Amz-Signature=9b3c370b6c8b7b5aad96132a2b77f6c1368e951186126bb078bf55223f0b6bcf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
