---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666POEOM7O%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIQDKNEkm9GK1qsxPH9%2BKxyQ8kDHq%2FML0sJAVpqsQpCXvrgIgegHkKp27j%2FI%2FF6VXa3W9bZowIkn4UJaQAqI96f3IVmgqiAQI4P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BUblcoi2EMio7P2ircA0pf%2FbCl6oQbfSHbMz9t8GsR9RaeEO3HBUgHwSw1HD70pXbf5VYJqkRk1TDzIA8KTQLPAvCalN%2FIrm7l1XhAzZS1nJHVECb4Zg1gmJ741f4yMm8siIhvKMZgjAWESEM3yMZTZuqHRhgias9IZ3yRdkYwAmysxAdaNsVhMPeD7XljVE3Vau4a2b6OzHA8KblJyjcBECUb%2BtbqZGK5nKZUDmzdXuVWMQu%2BShbEoFl3EhMb9mTj30PEXmMcMRcX7fKDtytlxKM1qONQz%2BTTp12oiDT60DX7ceNr6hRIFPUuv58iLFg9%2FoH%2BaHeqzOszWxAxKUOVJ2dof%2FZqc2XeCGVMouINMvZRq3LHdl4ENyD%2FMGI55WcpiUkTkMkraACuliHlBGXK%2B88jPd0J3AMmP8hHqReuSf%2B2pJRkrZZO4jp05HZjmC8KjhTEDM1ziQ22iMtBclU%2FB0E%2FJi7vz9LY1yM7yjC9czelmAieCG9l2qMbIX0PcmvK1xOAlI7omWh1oYy7Q9LJ0LaErh%2BxuUHCNVevMgRg9NcKkC2it10GwSnlFPtD119vsR5lcPEfO6xnAD5V%2BfE8RqPSfZSieS0bOVMLq2Y6HOX2N%2BcDjdJCv1t0r6FmmFx3L%2F9D7Xsy8kPZMOqp7MYGOqUB0OGizoTGC35VdOnnw66an2ZfaNV6INBQKrEZWLc%2FRnL7XQQSTE4HAIhrb%2FCwZKerKfo6Rxq1wA84nU%2BWvQ10vj2EIS5RvZWr4s%2BPknPOcSMU23VbcNjRuIanyDc56twv%2FJec0HycojQQt%2F3hJUXWw3Uhb3zQCOYSwhJF5RqZQuDoHYRXGnpUBANgq0k%2BuNaDXERC7DVj%2BgU5sXge12p7SCbqlP3P&X-Amz-Signature=588416cefd79e303f05d9c490ab1a0de4ec608f192ee8912357d96b975794aac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
