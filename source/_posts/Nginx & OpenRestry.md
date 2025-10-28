---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664W6YVFOA%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJGMEQCIFf8MHeRHd0Cg8Yk0BFGXxB18Rc2Ala3kKP5Th2b5yVtAiAa1AKxRTv8ApLKFnugKfTd6YVBPK7zRZrOzl4z03C5ByqIBAi%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpM%2FeC7ZrbkVHexQCKtwDoS5CDfZ2r3ttLaV9aqOvEQ8FEtX16tjDBM6fIe6AxScAgToGBOjFbMP%2F3eF%2BzSfmszerAxYkhCX%2FyzfOORGu5rihRdproxnKoa8IHe53mmG1AGssyTlzeE9xaMVh0nM8qf%2FLbwFH9KqEpOnuQiu3Ro9Y3BClt27m0tQAeVGBPu4JfdePEW52zDoYQgZ%2FqCM%2FsCbYIJavp0lsz09Oy86uh40juHGbJzE9zuqVuMUd3Sz0e77tebBa8QnAdoTPXMqPN2YXiJDH3571GbbgDWhoJ9zP5VO02YHHnvm0AeHcNPtccnzPKH4%2BE0A2OQf4mmveXno6KQp9ZALWh3wBn4BTGnYprCzR9P1WPMxMfv6Y285fKLpjxt7VdvgMRwkLPJZK%2BSkS2aN3H8JRMyzq6MQenRZ4KCAerHUtgW3Ix8pm%2BAilypt6ow1kSlG3dMRP4AegB6TFn00%2FkehE4Yd77Q%2BoZJGfbZAJ1cKSG47PbKmGCb06L5PJ%2BJSrO%2FSp8k16lBpzoTxtPF2U7RM%2BDGsa8Pu3xVDNnz6RgEmOmQUfqCNHJd7nvJfCap3HY0QWapX8mIxh1s%2FAs0MMlVJLmxbtJFrGPbeJWno9hhDWS0CmvQs8Q8RQ1canMpvREXmE668wjYWDyAY6pgEsav7ObyU0y3kXlRpYxVxSaLb3ndwqDHdfjoJ9BwPuQC49rtFVCjZH9bN3f8r9U4QqRioEM%2BY0Fy5Fshpw1a9f3kCYB2ubFO5t7Xp6L4vr5mJP5yMS4cGqzyS85aBZIA9NC9G%2FiZBZpyklt2wcm%2BeTpgbWh1NKYFblWlQpKbLERuesVWxXxVIvbpwIXrkhmUxDdsuXAR3m5VfRRUQGr1DXD0WiV%2Bkg&X-Amz-Signature=91868fc5ba523b003b2572d3a6c69dd98b0f89adc525891ba698197cf3594483&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
