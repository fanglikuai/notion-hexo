---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RE5U4AD4%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIGP5qHl7zNGVoU3KgmayMbDJxdFscBHHuGcifIY9%2F2NZAiEA6LgYBPVajtxCOijBe1NtcMbf0vIZ4Bwp3YX1cLB67fEqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOd2fafRwhYbiVjTmCrcA8VVB8eHaqj3EmeDIARIZXUuyGd9%2Fct1C%2BokcG2qbqNkZmkHIN8eeZj%2Btv3eztANYfnTl8Z%2F8%2Bz5RBCxITw04vVyDweR4t1TJQk8KCvTUk8nTlYAF9kzr6fMNAaE1eSp0wl7%2FZJ9SiGK%2FmEM6JOJFKBSYkwVJlAWGhe3m7jQhHbmdOxHdCfa0Wo8ZYgmm%2Fhu%2BoY4Q8KBPptRRIgDCdSEvq%2BljCxLSTuACOAiPUwNUTdGO9i4i3OvDlBQ%2FGJ9bYvzLmB4D8bnSq4i0pI5UwBsddPjAs7gF%2F4pTA5RDI3Z0VZqsXAoyE%2FCnHugh%2FlMoYzj336sEIpSGcxgTPFXxwtukoIkfQ%2BOaobdu5z1xI9fG%2Bag%2Bfs3i8k4HMX1d18LUSfoJ6bP1CkG0c72rWwgDCOaL3zQrzRTVZ5XsING%2Bov6Gdy3yBIVuOVVqTZph0iKLElOjBkLqeAP49dbj4LAm76Y1vEC201Ot6jUHBZVvxn62hWf1SZZyQyZ5SM%2BduRhA6A84QbCa5mTJFRBozzcbpuegvLTwE7AsBg%2B7WxptnerjERrpuPVXC04OvOL0EiYp2CNczK9qlxQvIaeRlxLswxblwAKNzvpkBxoKg5VTm7ecjnHzIhePVvkuzGe3aI3MI3ZusgGOqUBqTlAceQKaNCP2%2FDakTTYid3%2BmBaP1kPcr4ihDxx%2BZoGR89izMywpJCw%2FCVTpbYkV47vUH0x6gHP%2BJ%2FuTUdy0PAracMVjcDR5IuogZQ3xotGi5AsZFQcbrz2XkWURarWOLFDcH1gnKcCuYyphxe0GHvhJT7XtYT2YOf4cF8B%2BgpJUBe6V4RXeuGn7nTsHRkwDGP9C399TgWoSGjU%2Fg6C2Nf4qte3N&X-Amz-Signature=9cac0d9c20153366383aaddf9aee1f288836f51e822eeb78bb15164df605482d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
