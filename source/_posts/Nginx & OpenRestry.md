---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q243JJVY%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIFCahMuBUkOaCokFYU424ZMcy3yos2Rj9ups%2FPgCqdjYAiEApi5aUqT5GBYC7Y1Rmeqp1QBGzylxVhxVTKUUamhGGYQqiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC6GkA9aT1OGTY47FyrcA23BsUd3z6lcXdbyv0neHi8eA1WiV9XUJyL6ML%2BRCY66bgJt%2BxTwEl%2BmAKPMKbRwouJDQtgUEPdM%2Fl06BBZU5M4tIVTyaFxIryz48NcMiq7EyIWm0S1bIbjbap6PtxoZkGxpyU2dcFuwxA9S70Z%2Fd%2B6W7cTD%2F4qBSTYUuWre5i60oUt9NSDPp1S3%2BiUQ1HjWPXdwnPoW5Omm4nyh4eRF3K%2BT7zXr3tq80lmJK%2FdimMhWZSo7HePgVXRzDrfVsftrczQajXbXtgBuBqwJNt7eY5MKjh%2BoJMmu2PX%2B%2FT%2BejFRi7OTlotkuh9d%2FwQa4j%2FNTIJIeKyDTrvy3mMm7ILa3baxeMkG3kVN3uM4d6zbbIDxsy6cOF78pk6R2V0%2FnT8gbv%2FAukHx9SS8lgt2VmQPCKT7yHdBFkbeZHCtHtMpWrjpgBSoIK0%2BOdESfYugIewFAui7fhp3dC5E10T2d1e5ovK6phIWUZz6I4xMutXEp6TWd0L%2BhxBT6EJH%2Bz02tgn6onOh9HOgXnYMdUfB%2BGQxT1ur6whd6DiP6w0DdX9b4%2BFSu5wL3JYfJaBCS6i52Ts9pZUWri7rdG5ohdNZWk6kwpriFlYVaZ0%2BNxAwHXdxCRlzeDSM%2BBQIyI6SePPBvMMnQlscGOqUBX5puYZ%2BsAx5JUnctTdKYfGKwukBdP8lAVAoIwqLXKjg4hdE44TH%2FmVaOfOa78X3ByS6%2FlUlARW9oLBqnpCSutCglJ5EsiJKCAozP8P45uaXsUP9cx7OQYxpVqM68IFx3%2Bi3jdIeCwRFVQE1yO3BRneZJipOu6XEft5S8ph3KbCFTfmvjprhpL8GCpap35um9Fx7Qv138kfZZYLTo%2BSCFqqi%2BObEY&X-Amz-Signature=0b8164b650604afdd7321d8b6ccf7b81f4300d46258335e87f42ab048fc6ace9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
