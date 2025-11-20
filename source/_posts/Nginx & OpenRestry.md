---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664Z4O6DGS%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIFQpleJ7nVFFm7JgkVHXvc%2BtLUJtB9xHT4crvHiydJ1UAiEAtFoBammZzzj94%2BOsrN0RYv2GfmD03Zk6o%2FnHl1TVs6UqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPcydLyCfsH5oZbpJSrcA23CPKW5WIsEWTK%2BYfmk6yg%2FWV4oG36bNWjJfLiAfGIMCnCpdu2bWGrBpRtoNgNpSEKczR%2Fj37EEUeSMWSnwI86Fus9Sm8yidG1%2BPojNaELfRHberjqP400F13crGvILFPeTQ6nTUn%2FZbcvgPWLB9vZvUKCaBiTjsk4TQ0Pff%2FyVEVJqcCFp%2BKxPohW0Rh8e7%2Fy5knsX88wcNpZkiMseKRaEtdHV%2FSYxTsPP6Kj0bEL1nMmTQiD%2F9pgVZ3JuRbJ90Zl5jfhDkuSEuwW9MOwKpSyKQpIeMCo9g6DY7D%2F2qldrC7skmULvXruvRr4tBtEnYeInP7g%2BstTfqRaole%2BsDqaiEKvICdtpG6xHmrDm0qZC0rOSpArmZ37OD%2FeS4%2Bzl%2BbU3llq%2FMwqdRo2Zur7Go34yYZPqNbBzx6h7ZUb12CN%2F1T9zz1wUneyKyxvdO7yAMAee%2ByVmAhMySDI97wbVOdLuq%2BgOX3QDpS4qzxDW3vTpEUa5K1tmrV1wza3VpnZQi1mE9Kkw5sdxMB7rVyYI5iFdi%2Fq5dfjxo6grnbgOEXZf7ma4c4Qug1D08kAKSmNnJiUXYCTLuVnXxojZZ61m5QEPnb%2BrXUANkBybMchSqj%2BxyeS%2Fewe1KXVxsDseMImK%2FMgGOqUBHqN8gG9aWSLwG0ugdBg1Rp6lRFoKEjQVirYlRN%2FjGjjgUqvPUwcdy%2BO51o5VVmFWylAbmyrznas8JxhNKk8VMr20oIar%2FpzHLQJixq3bZMGMEIqeUGK6rMFKoX81xKKnGTVo9Ub51At8ezkzgAShgexI%2FS70WFgS0ZhiymmNxNKcdGJ%2F0hZObhdo4%2BeVy8X4VA6GRl1ors8ayDLDAiCTVmvdGM03&X-Amz-Signature=a7972c9b399675b8d738ba77885e5e84dc9ffaa6e8785e51ffc16fd290cce33c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
