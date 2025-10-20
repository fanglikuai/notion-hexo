---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3XNMCOI%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQCt6%2BBA76M%2FymIKu0jH0LTSkai6m33vieNuu9MUMI1mQAIhAPainWBSg3hX9ompSCdWKRKsnFvRd%2BFARBzhnSJsglOHKogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzGDUi%2BQhMN0JbPFV4q3ANrjyq95t2cnbX64OoD21GKAwkgayLJK2IVuoDDor3%2FuwISwmaSM7LtLWUoyb3kKFeJzH3CIRNOBi6EsKy3jU5A9Nn3cqhX1BQwbuRHmuQqws7PbQFUYH%2BmcqOfYj4Q9w54AWoFh%2BQH88I8FpMmMvbSfng8tdnlRMHpjx4nQe6Rm55fpigVki3xvra%2FalqpJbHFBghhWqa4qVKYSPIFUjWDyoqpptfVJcbA2Gt2ws6vACrvpE3umXWZikw%2BoGSlDMy2sA6UFXXGFmCsfN8blNdfGVG1DWsKCFWp2FSIQ6X9ileM%2B6DL5iUoUGxKEX%2B9%2FSTGtlQXkV2Qa9YUnVMgiDYYoFVapfZYN%2Bwb9gUdFXCeT%2BPIRfHprEV3%2Bwg6lyYFjYTSkRmqNKo%2BL99siOO8cSj4oVT%2BJ6u7UsXyJTUmqGPxpSm0MODrf75lM0gNuca4sHCfM5EKzCGErneqQ6KWDycdm9v1tHwHfSUw%2FAdcGqjAw%2Fg1eRIlhRi3g83TnvxhOhDTs5Gp49rWCiuC5i0hlY3Y8mfQC%2Bq3NGp5%2FMLc%2FSJ36ckdBL0UtcPJa3QYs3kmYRSq2sCUsUxYBY6qPwo2GpCLl1Q1KLoZ%2B5yBeFzgt9%2B%2FycPZazFO3m5kfzR9fzCg99XHBjqkAYHBwEDDRRVl3BujkkGA2VzWSWv6eoHxRtz1dz%2FPsOmhcXvU3RKJbZaErfqjO%2F%2BCECAliuMsSxvD3eSfPkP15%2FwZBSVQRJVDAbGTOq%2Bfqi3CWEkDePV4TuX8XPbtPn3iy4uKxktuomXTClD9Gtsw6wHPtKI4VANHvc3Qm0BqP0iwf%2BZ%2BHILR6ZInKZeDl89fqDE5rdYXNAcCOlUkz5x3lnLbXHqM&X-Amz-Signature=b61ab71061b08c95d167ec5bcf9f843c352f495a199fc8627bf88b63ce2f3461&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
