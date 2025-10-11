---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFO2B5FJ%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQCNCH4KBfgYDoHuoVnL164xxk6Fn9fJMSFA3rPd7wqIEQIhAMRQwBJUXw4367Crd8vzn8pcScOMXsr4BiFvqBainj2SKogECPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwsOGqC6nOHBcvgZlwq3AN3gJ0it9FcEOOz6Y4LBCdqWcScD5J%2Fnhoxpbu8Iz3BrIO8NLEmXatpOOmPgbXscXKK3dQvR1W%2FN%2FAZvBziXgnBjwBppb2%2Bvk9MTPyYkfVjqlm78JkkVjyb338wML7xkamcKQ9Q0nkgI%2ByMkW1I5HG%2BqJL%2Bk3TXCqu7bOaATZ%2FiRCgUYwNI4o7F339mRGGLrlJAogifGq8L38zcLabgx5qKWnouyMdbb%2Fn0d97JtvK0yevu74JTYqe1Ejsw5YII1Q%2BWwp97kfNkLm%2Ft82eA2C3KUirV186QgREb7bZbPtx7LFwmBaJCmpharPDbfQBKYMDjwziyVWr09nZBy0%2FaE%2FTIddbFZ%2BXtko4O7MqP51CRMbhUUCkImPToTYOzuc05h3zXc%2B5KwSwdehEoZUnNOEtP9jJ0fL5N%2F3S6%2FtWHJe1lFpdEwQmH7cyXeGX8xU4eHft9lTePhpbiFXysfgT7SFVAaVQ89qNd1hJCduM%2FW3cH4yF9BQTZF7C6C%2BxyNyRcOe7emBF5qgArTrwvxqgd9q8sJ5TXy7%2BAi6Ww2nibs2UNa5eomKHLNEYyxsEBhtG%2Bqyn3hHVSbsafOVjLs9q9WW2SISnzT3rs9figQVsn7TxYXph1CLb1FU%2BvCXA%2BMDC9n6bHBjqkAdfE6N%2BK2zT75ebaEaQJrx7rC%2FWSspFnx15cA0SJQlfJ0QtRfC3EYb7nHh3MPnhfZ%2BKK2qw5SjyvPi2ptlrL8xGQeE8tYFDbb56zwEhTyfuVgIKcAcT%2B2n%2B%2BWhehs8y3yP2Hn4eVuFb8aNxSRoCKjxrHfDiwqksgMebgh5rP2s0hqv4QbE9gvOs4KA4rUH9ctJ32ilhgN4222hcCGXXF8%2FSHcWP%2B&X-Amz-Signature=d456dd1d86e1df9cfdcbfa5428059cd8c5849cd64e0a19431ff8796f32c12d94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
