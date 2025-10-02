---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2I56H5A%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPoRvMRoxqPDxaSYLXjjWfAXmEo5poMGiEy6Dc3LDgqAIhAIBX8FxOcRq%2BiwzD60FkZnJ7D2rcRmPIBVWISben%2BIC4Kv8DCCEQABoMNjM3NDIzMTgzODA1IgzC4TC6vBgi%2BErIODcq3AOlO18MY4cgVE3WAlHTm90TfDI3ku8LINRFlrfZ0Zeh7o1%2BX2KdEh%2B%2F48Yp5TfWUWvs6RH1yO0L3WabAmXeN%2FC5cYvkZlbgK1j6X9QY8FMZGsDhSGl8gPDLO9CplhNRj49u9uXlyT69up3%2B50kuNFYKqF%2F1hiUPzKdW1RqxtC9u78vjoBU%2FgHBGpnCUlrB4t9Fa4SjlsrhNdCITV6xYKWal4jZ7pg2GQ5oaktxVSAiRYiNkbETZy2ZWgnalBg6SurcjMBOfy3P4gnfzTN9566zcYsK76o1iAOEiJb3bnAUS6k0geu3Sa6SREQMXRH4IbcPrIQ0GSoehV340fvUGmbSTEvS57iN9DGvyt%2BdV18c6iOHkTeXEYdF%2FfaGVhUz1PPrUBv0ewJRBNw4UlVELyrJwYNb5bQdzWjF8gqlBEP1ukemK3H%2FOPnVPdoXdw1282bVTrAJ5F64H1tC%2BHquSMsk5tx3Ssv1cVnym%2BwyMDgm1Lk8%2BM3clQoM1yKGHCefViYCU4AOPCDeGXrFR6q92JvWiLCWtEmt%2B%2BCou4OZxJrjlXOCRSpdIevsp5Kq1WbhDje3QURIjspnU8t%2F8uptyJ3bMJ%2FBSKo%2FpR%2FAiY4CHjQsry8Lm2GdUNW%2F5W9O%2FkTC7%2BvbGBjqkAWPUtD4TeDtbScnPSF7nzeTMt4hOsqmfLQGBrg61XnqtbEvNUEHc0OdJlC6euJ0qGAv%2FcUc7FAVFi2UHPriTLkNXC4qi9iRi6psPpAn9ivvgW67rREoPRPwP6ggIQFBQ45yuOBO4yM5j59p1ywhfMRxbRwFd6FjDShNODdXKgNDBi4joUFPw6i9IMwv4%2BlpHvh1vjDt%2F8cnojGZ5e0wMx8Yj5wNo&X-Amz-Signature=e4fdfc698e2d015488c767131329134e96ca69178960e75ff4cc64b52d41f8b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
