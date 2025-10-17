---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN27GLPA%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBmiX%2FJvtU5p1AmilLDZW4P93B8XtkOX2tLdHNwzwf21AiEA968XfSpNq6ohe%2FDtq%2FaIFhA2CazWa%2FF9361%2Fo1WTs20qiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMaqT4vqxAwFLeuwAyrcA9Kcddjz4vFGg4BUrmdrzyR28Af4WWpf%2BFwKP5sRpLDXyBKM43qp6FwKIE4OjiMMY0jusxJYh1j%2B4bDR33qo9n0nxG0%2F2hF3ICYsF5TtN0hDENg71B16M42J6C5pSMRuVJ8P%2F7MQ%2Flz0U6nW2BwKhdQBG8l%2BIq1NNkLMaq0wdfftw%2FzcEkyT0EL5ZWIt0htFTTG7EJjH5JJmfvdarxYzLtkQlo9Pw0kiKHiTAaNB89mHzlLves7ZARuZbzfRPo1cy5hoA3kivPLXd%2BX9vSCJSpmahXOwYrvqpNvXPmIDW4YewWtSrm%2FasHMd8yTL9xTFJ0z01Oh68EGgcKvlrOZgBqwZ7MOhIQcRxTq%2BqZDbj1bqvcTUVb0xvdBGA3mT8jie%2BqzRm9uIYaAZ0emQyIG9wNmPa5nMJY7LyjAwkzV2TQQWrk%2Bq6KFYAmVMSKXoejamJU68zNfDSS7AMKk4hrgVcaDnvTV7bT5kfXnYNAECubHNbF6jJctKijUFiNvORpH0g40wbnCH1JpZaFdeo7EZJF56KzJdBZnaFQBGNYgvYh1E0puZrhTxnlFwLQwr7hYr6K4SpIRflMxVcVRQby9nIlIyERc0TW7rihX4UGOj9Kklk2eymBMJ1AHWMZkzMLnmx8cGOqUBzuaahDlMWIUuzoPAXEj1u0xaqqg03O5teN0FqZoV67ahYf8DQSueeacZpZgCacj3XmTJfCIBFl7SCfZ8Gh%2BNeOtGS1j1JDbvyuPFMBHi%2BRw9zqFiTOKcg9Vm9UfANMwnguF3L%2FzB8OySKBSqFBYKkR9%2B0Q7wVyML9hQDEt3A%2BT4QGqdyPBi1BBDTIoFGBJeKpHQO3sIlmIrgr4LBD3poaQJJcdAa&X-Amz-Signature=90775b912e83b4b2df845a0f9312da920f4011a3f45cc364951489d5c1a90ba6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
