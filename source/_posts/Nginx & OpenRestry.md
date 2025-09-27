---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TFG4LAJ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIB0J1JyBk1yT1h8DUc921bjBxnMsLcdhLPR4QOnywYVAAiEA5WjW3S2OWsVwP6PAXpZB53LJHDNjkwmg7gNHRdK6qWIqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBG7xPE573zwhIk3OircAxDrjqpos%2Fz9rlFM%2FDGv4R38W4VXdVuCfXhLIt%2FGg8GbTLEl%2B2FP8lsJ3cvL2NqNRU0z%2FJOdagD%2BZ8c%2Bgo07DXrsUXWvflBstVQ0dG6Ljb1jCSXVsPkffCrvkkmWmTgEqoypIK%2FixOgp27VrWVRr5oGDC9xUFNJ3EMndRVOLC25VYQyCEv4lptricSjfekrpp7100tRF1I8Gi0QpbnO1emSkDSeKqiBq3yj%2B38e6ilQlSRSmo6jtPTOhJNksSTvwxx2Y5YcWPInq2mZXx1DtMdgX68lxaPZetU1yEswYTf%2F5PfGKiNYUEQ6%2F8%2Br0JXezvLMAPINciikTADDvpNVnjA32vDs%2F8MAq01%2BLDt%2F9h%2F9vcU99Pw2mNNRP8%2B%2FBMaZRJJwBUfNIErxr4ifJhDhKarh1422eoQbJYBcq6gl694%2FLKwBsPCxV3i7qQcn7IZAMrpaymjWVArXLcMkT53PXvZTA8keaDp58HzvMdNbFQ4KaykM93AFBUcuxqrK7tO0l2lC2JSjfXO%2BMFEXrJATshoL1xV%2FNbAOsWIfavqMXekRwUbPtYOrV4jvkAqe9DU83RQ5gxPfbQ0y8Ubojqn7Jvr%2Fk9uqYADaUAWmFwnqC%2BDJm7yN2TCGlfY7me60WMLHc3cYGOqUBWjLVvShKcfGtDWBMIGG7s7LhNkYWybb0nojnnDd8ziWbYENtPou1nJnijOWf%2FnG3%2B%2FyPVGfwZ%2Fv6oAq92JyF2qAfRCqJQzpW9AWkK8UtolDnyIb6uUUIJXbqebaKRT5WAbhnZHcnuUX9G6tCg8yLphgEL4w1N6LA0GPDlnO%2BDy%2FowuZm3TGFLp40eCMg3usOq5Yco8LG5QuP0PZqYpcf4JbrZcpl&X-Amz-Signature=d09734105d272cd505bdaabaa7dce0c2416832ba22cf707004d6b1392a143ced&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
