---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RNTAT7C%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T090104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCf547U3zbnlXvF1GZhA69wdhzU7qG%2FEtirVcKRPVOINgIhAMrk9yomXXyiblDQUiNfTcVVWtG147vIEfe4nxfHrdS3Kv8DCFkQABoMNjM3NDIzMTgzODA1Igzh6Mq8PYA9UBAXVRMq3APfq6TMkNXR%2BxJDGnBPlrrrW0DijYNf79me7EJZhpt15r5V3OePo4dhlp7KJUMr0Tuzovjia2vRT9P4RGMfjV8czLAXrsCWoLUMqT6xiPMsvj3%2BGSLblqUaAiRO0oIb5lOpW0UO1v8IyGgzou73rZ2HOAFOjuS9SmACCV1xpwiTk212zWRLtxL2cwEsr7eulp6qMwFuFF0GdphFkBjzp9y%2FMvgNL%2Fj8UlptJ5wSMJW1D7z0nSh3WnAbP3VU4Kayj%2FyaVUv6ak1seFpBko050jD8gjmRx4wssLbx89UQvqbF6fvr52Xk9KWCHlidVuN%2BpTsJNGG8%2FiY9CCZ6PHf1Dsd%2Bb7ZxjBnp9MNQCW7Nst%2BBiR0QKDHkvAyZBmoE28epBgRcvUI7EmymcTPLsf01rUcN2NPyZstbFscIVtWZ9h46ZtKn5VdmGWebk59h2XQJzl8LCW6mXvq94guH9jgTAUf22rADgU9bPNC8QEfuAzR%2BbRcyzQVHAa8lJghCt9mPBrsxnh76AGWIiHMM62UDfrlYtvI6nFHoS%2F3vhTiuBZ4jyOkc89WdaBLiS2k41Zwnvv5JItgtUBpJ58R%2BZsa%2FqjZ66C3nqLNcxAZ%2BBV1eYlL6wxueZlS2s09f7wdz8zDfgLjHBjqkAVV6Hzn3wqtuEHQiWABOoRNG9mAYUObOZc%2BVp6QGVcAcpLIJ%2FWZVw0LqXvOG3zV3bPZWRQOLfNH%2FZbsbeYakfE%2B%2BlsFZ0I5SrQ7FNRJwuNn40t%2BhIr1fWc%2FzP0PrXSXwzVKclI%2FwWZ%2FHVba2xKh2oEKuQqJd9Gw54mf0pzTtbn5fQiEw4WH4OWMoCP0SdnrDNdXjCwoG8MW9RilvW8knj1RgvLkc&X-Amz-Signature=e2e032f30c6c62f2e8689dccc5dd164168ae106dd8007f78bb97afa90400600a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
