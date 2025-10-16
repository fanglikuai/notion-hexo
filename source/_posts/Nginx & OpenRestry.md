---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LVP3YP5%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDBCJm2d2L0%2FZm5%2BqOMevPMb%2BkHVps4l7z5WeLF17qr4AiEAtbJ9TCyVxh2SNYkCods2gt7Ccy7TNkHXiv3tBqqGr3kqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJohYy7YV9lgedFXryrcA05sNCO4B7lI3t%2FA%2BeCPhJMNCnKYUy%2FGHcdr1LDYXJIL2fZyuz5sxVIHugp1eqxQtX5skGrsVPleg58dEKXRlwTOpZfLZKBupij%2FgcTpiaBOsji807nnPcbPz3zwkGuQkBxH7cAAdfdDEHfnaR7BDNAMTgq3nnoWD73ED6W3J88HkKCt8LgUh33LcEUr6KHOReWaP7GMK09ZQ3SykqJkbMzcZ7lzIxlY7zeu0mZooQMSMV0oCj8NBRwEdqfWVsO4Zdm5I86hK3A8XdqPAmhWkCDLVt3%2FCBxJKQvktRY5A%2F%2BVRuzLtQmzBNJ1i2QZN55Mli%2FNBOD%2Fs%2FcC9WrYdXzPH1a5rakV7S9mrd3Aq%2FzFxc055S1zXqRdX%2B8N%2FLGUsuIIpG36qXVLgbuuEhaUU2QUXoGkKo3YVZgjszWSja99vlOUOxdav3eeKSYYL2bWqVLYB2TNxd%2FoftRFTfiyvnlIO2G0vow8SYWy2vbMW%2BODNYGXUZaz0Qn7Oz%2FOIyLoWomMhEBuYJSQkCU%2FXs5VvWxyqrCmBp8xRpADfFQBIWpyHjhgJIoz8jQy%2B%2FSSocRUpoHnzmPD9zAbt7j%2BRb7tX0npNbuYzbq0Qyv%2BgUoxUCs2gozDuhkY22Edi2ADJczqMKXawscGOqUBdGsi%2FWvFKWd3N0yYDuBvRpDLr4Il5MzjINBqzGvI%2FlPOdKwSu0nLOpxC3bN11n5oEY7MmLO3N%2FsYX%2BM7%2BdvWNHqjvpdIpCTX3n9tcb%2B1r2FeVETolG%2FzF73uuwihjwl6lagTawsWOEqA9gp%2FugEXInPo5goZlce%2BYH1U7CfqeksX%2FApNOsgl0sttErtMD1T62aQM2nUaI9atva5V%2B9IuOqndKyMi&X-Amz-Signature=60d0deac27bd8293c512ada53c4f02d740f6e75c17e93edb910e5f2d882ea1f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
