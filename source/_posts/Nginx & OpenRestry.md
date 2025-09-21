---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIKVY5HG%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T080110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCusF4A8DahVLko8qltw66InqYDeIaoAGIWxgf6iT0t%2BAIgVUxgMHlNHbRUqos%2FLDzn7Qo0GC1jg7%2BStO5tBAMeStYqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB7eHeT4wf71YDDCkCrcA6deQAlvS8s%2BmjyGYM3etwBITw5ORyhMsoJTnXuQ%2Bw7sbNRlp%2BG1EMPYgP%2BmNCGxyzfdoY22%2B0bLez0yckFDNO2mQw%2B%2FoaAMEWopeaJtrfr2hEsdeCX8TfVYe%2BCZ8fCsQifScxeb6ox7%2Bu6nq%2BtJXFoCQZ97wxK4jkaJJ6dd%2BWkXI111yitr1kNz7hMfNtEkXf%2BIjEP7xALIdG5Oute2p6E%2BBh4%2BkN8wnqiyYgXFN7utWquy%2BksmYlOExWcqtDVDjJWGh%2BSUsSZMKkU5yH0HC3eKlRUgDvCWTlj4rXH2hcXZn4r%2BJmkBfcGizG9o9xMPReRfy1FZrcTo795iloDvrnB7UkQ4nZlVXUY1fi6CxG1pQDi8HnKYoCoHg5T4jboeRxooZnWjYpaDeqJ%2FPhR5Fed%2B8IsF1Q8%2BzNjFXWbaNC8VAdlTLEBeB4ZRr9qgyNRlGf0qoctTcypR5h0sk%2Fmz%2BZTzRsnjLfyeFxwC1Khcyp223%2BrkNMZh0lt3vxtdf92gX5JfA1kKJ4a4G53Zqt9LxSZSashXw5k6Zr7rn5bpVUMUprRl4XlxBSmI3qEuVbYkB5XvghZG8Kg%2BcXQ85uJ3Bt9G0nluSy%2FTEJkTGf9Qo8Aw0YA0iIR9AW6QjreQMN%2F%2BvcYGOqUBF9Q9L5WpAIkGhXkTbhubjVbK6%2BUgEBm4Rw5B9YzWSfSYO2ne485bWFVb2UWqhhLuo8qWY5UobFHav3XeAUC5PXjzS1iK43cCjdYh%2BojUUxXqT42NdNI6FzHA%2Fs2gaasnKqISU6dzGqTv9VpdQpA8NliQUkX%2FhVGRzXkRGe13RQntUHklM1o1x3AhnMVDxKHcYY2gr9AtOhf0fJyF5jOOsCS%2FXebT&X-Amz-Signature=60854ec811c4ee1dd2bc81660f49e27e2dd594a8ae1689fb18f9279d7669d7d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
