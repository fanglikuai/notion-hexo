---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ET5XT3K%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFhNC%2FM3MtptwsNZvbcFBEl%2Bjyp2h%2FVxr%2BdmOrhE%2F%2FsmAiEAlY9e6u%2BMgADlyMGE4sYgWfooKyWb6au8OJR5xwvOmWoqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOkTGCkEAZ1bX0mU0ircA2SHsH9h0XAPY00r1o4Ftl2QbZ5UGxc7U%2BQt3nRAoX2MqfSloZzelEmSlenOKxuJqv2djrdfvDr14vdfWz0cL%2BV8xcRrrFtPkBmrGUBH%2B8bk87%2BHk7SIU%2BGF0ciOHpMW6GggILchmKO0N2BLAlVRUpzZcvAyxx%2Fo9iaxcrNRitXmdsCL9n%2FjkZ0XC2fWRscwroYD9eBaHJaVrD91Tj5W8UhcB3WTXnu%2B%2FoXWorRf9toLx%2FGhcjE63x25L%2BUK9Ga1ulPnTGgsL9LDVoGm%2FmD1cyrO%2F4r6Ao2Q8l%2FiH25ER6w7NtyaTa6v58bymopRzFQjjl9%2BlJcYO1s5wcGQiaa%2BpB5hO%2B2kr%2FKla3YxYauBwcEDsVQMLde%2BFnER3OeBfJvRxzYeYMMetr9STLNWrTHX97mVx7PctkA7xRatxx3UV7Z6KYPtuXccX5Q%2Fl%2Fx%2Bd5mhLh00wNOAnueEjnNAZYvCL4qZp%2BjA7ahh8DlHfrJSvemSyl4pDAN1SMScBh6%2B4sw2BaBfkCtInrDoetZ2YZaDCZCmwtEa1xibxETYl3KQzxI02ZmaEWtR90M40E3mgWO4z%2BvuQx0x%2BxzklZXe7fOrwTppPsSF8XWvCIa5%2FReu7C2zgMz7iVh%2FDHobQ6U2MK%2BOjccGOqUBAwwBV1mGRNCppw0nKn0xchPHqYsjfR6VejOYiWo%2FZ7oe%2B4WuGMJAfPtmE0zaqnj9tRxfY1qtb0mkcsbtC2FGatUdT%2FKIBedlszsG7XCPkto7sr4fz4kckNhqfT7d%2BH998d0YGRR4Is5MuQHuAFMCxTWSsHZbZYnhBnkvLX5XvTgoGkSCmC5gC4hND2ujttrMo5p%2Fp93bcUqoPPknWRfVtIhAMXBX&X-Amz-Signature=c14b08a1fe895100278b4ba248a58059b39162984247706bedab5a80e03d17fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
