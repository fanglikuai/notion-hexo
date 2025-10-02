---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDGUC7G3%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFwnx54920F2HTKqBZrXx39WhaJ3fAPMzicevrW9x%2BpsAiEAp3drz0GUHXk18G5ExxC0f8Dq0XPeCtHAt%2BPiwttw8kAq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDKY14K6lJgIhUsoaByrcA9Sy6HspznmLtsTmq29wKkKnVS5mJcuZFd21SnTCEjtVKZN1xF6Z90aWPX0dmlT80UNjYGWsuDf4RoWyd7wfrsDc0giCNxS0FwOw3lRfYMG2MW5RWKLT0FSC2suM%2B3gebZzdkizq%2FNq%2FTDTo%2FuIzqpXwcMEt0gvUEhbzc3Vb6QoNG0MXQ%2FHYzFN9ZJjua2FTRkpWPCiGtao9UciD5VUv1oAdWzIo43OgC%2F2g4GOrB3fqSXYIA6QYpfMIN%2Fjgx0X5FBqueK2UnAJiHqpeeWKjt0FkWV6J7%2BzG0tdJ%2F3p%2BTKofuOv%2BSVPqBPTMAR1uHncuT6eAV%2B80ZaWoVhsF74FTEH%2FRXOjgEDmjO5dDExSHKU4RbjfhrR2D11nlzDDvC443UCmYEf0W%2F%2FP%2F3tYm%2B2DKPFxaCYNRnoF7iA4eInZE7AxNowmqPmqWamQgdKKkvIeEjLUrh3H6qTnrmra%2FFyCJDeRUbNj5za5uDbVnjmUu%2FvZjzkFIOuQ7nJgdADxWcSMk3ZpTNAz8F0KawlSAArK9fb8J05orFWwqivhev3xdHhXF0fLIazYeV2XTb4nzJE188heUBLeCxnMQVbgbVKpSoYQ03OyhCxAeY0%2FTqBbWU6FfIG%2FiUI4ZelYdcO2lMJOQ%2BcYGOqUBp9s2EY7kHZQfl%2FVNxVE3LRu8PpBhmzBujVzNTvKeMtrhOshd0Aa7qKPuARHsmDjbvZiBbpJB4yP5582HN3sFbNL8ZBO6aVBuMaxLVMzaz8Jo3MZb%2FEV2lYsz4Tpdk%2FMLoMOV91jmWI5j9pg6OdQbMSlNto77ZJByJY%2BGAn0ubmWmPBnlKUOAGSYN5IRvB8Y7eYvAyvqwxLCc48v94255JCoPT5Mb&X-Amz-Signature=826ea24dd674160129d9b8734eaf1bc0297a6d6fe2c4d0d5b652b416663665c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
