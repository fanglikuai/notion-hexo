---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XUNUB3S%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCanQY%2Fsp1g9IWdhEm2%2FtpdYDnfBvB3levGoYBTU2odVQIgQiGDz7U4V8u%2F3Kdou5LMvhjSGURb%2BSE74iGU5uUVK1Mq%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDAe%2BKzFEL8gcWLa0ByrcA5vAQeSIzk22QTjqum%2Fs5t5FoAHluYmEi9urxGq0Kwf6Xv%2Bxlma91XrhZwhLraUOXR5FVSkCXPu7w78I1St8nIrYlnNDqOOkr%2BAlRTImFb6b6iE3ED90dW95vLMy7559Mv1CePAkwcsuA8s7Vjr2rVtwgwSbSxA6r0AuWozQpKylFAEsGKw4dZqbqGuhBoAySHenae8MhDdySyNIy9oBY53pUvmZEbK%2Fm%2BmWsQ27uzxLZaT9dPGLAaso5GDkvIswuQI0O2cKBwifaEeX851xAm0VNhUOylSescxQBeSe9VQlX7qiFkx0Rmy7sIeA4mM1FSkAlFoZwqPFYqlmwp9H7D%2FAGNUCux5JB423iigZKwXnkJHg9Rfh5FtyTWGrVo%2FvjOVGXBRxKpyLcxzjqTmWUdosXVFbFMI%2FoS0NnIvDvwehLC8BfkMVdzqkcyKsCqzyXmbKBPOlzXGvy5xZgg1d7Ii8HNzjpRgVDz908am%2F05EdK8243kO%2FGVXbbaPlUXldKAn8VmS8OAesaMgK85aNzBo7Z9%2F5fKfhSSJg%2F7c4e73TMzD1gNYHbG4mgObh0PjqXsn2RjXFWxcQvicRDNwxR69LXvikt%2FIoSEQ%2Btt%2B%2BnwWteZz6lfAbMNSIw0%2BYMIufycYGOqUB1TjwjISG5iTzNAylGreuyrgRD7qsDjGClLewSoJ5INmqxG7JcnIoUp%2BW7a4Wf3gf171CrUsjCcH1kppRHFKn%2F7gxiQoyy2bSIZg%2F%2B7oUMFnNeOKYj5QuUKxXaQZd730iFHVRv%2FCOkCPIpz52JHW6lXGajTCxVaHKG%2Bo06bWj6%2BJxegqY8pGe%2FqqbM%2BNOGbSdRgokvYymfq8inZ9qqAdm9SHry%2B8I&X-Amz-Signature=c433a35e7c76093e8e78eddbe6164143730ee7d0c2e997455bfea64d6fb92928&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
