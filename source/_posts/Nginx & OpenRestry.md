---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662O7TMRVB%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIQD8Yo5hH%2FtL%2Ffuc%2F21HRv0jGUm%2Fuk1XwydeRtZNR9%2FGkAIgNZl%2Bfe65qMcZALZlwfJpQw8DiMgNYQbXjyrZcCH0eokqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLNIlAigwRzllmlaYircA3xa8bP2cW12bBmzrHJC6vz3xabu1l0YEs4vimeogBpFhaaJVDe7qh46wPodXha9vjrjdtSbHCVtLr%2BJJELawkcBlDXoTYhC8yQyPgoa4STObhcKtpwNrPeauSJlayYGPTFHhomK2WOGIjCTyS9KPAHyiqx8%2Bhs53qVXpbCYoflUlS3F3gTZ6F0I6EX%2Ff7CcU0no44iLrcbKWvw0g4q0UtxU%2BbZXP9F9qxDsTU7L%2F%2BbFn2Gv%2FkBBpEh5w6MIleqRgP2g5hC9OJkXbIDtSb6DQaUstB9Ze6ooJ%2B6%2F49nNQFTFSiyC6ZNa21%2BaBvnWtUEOYvcYKC14WKR1GjVmWBje9KOf2M8mAeMulwMXaFX7LrClbJVfSdz0X%2FiVaTzPmU45XIyv1XcSad%2BxVrya%2BS7qwJbkkRiIHLDBIp8uRC651GVAxywQDakNm%2F0cawBQKZ%2BpIxA1yu%2B7AN7beO%2F%2FRyAnGf7RsAyS7r9DcnsdU%2B%2BQb%2BWq3b0jXK%2F4olzUegw%2B8zko4AYommtZ3ZEZv%2Fa2D7hMJT%2F2nifZBzWAP1PfBCGatXavpeNWruEm8MA8RepARL4fK6qPAGRF%2BIp0YVfYmwjPZJjKcNCowzGUWKmyLCnyFK%2F3L38imgds3ePbOylgMK7oh8gGOqUBJrm1F2ME%2Bd9v9%2B4QIk%2BF3POTziMpyU1JkJ33GisfdF6x140gmRVVWXpeZQbvMKvCE6qneXekWy%2Fb39HEIwmjW0Ndxw1RB4VKESE09bIAZSD5wkdrtb%2BOpENohZJybe3%2FWl4%2FJPIFLDOW29s%2BDT%2FNckGk%2Fp7rPjrT9nvrsRzQ%2FCEGerC03A3ksas4otFHUKJC9f%2FnRYczmrLT3YLCGtxnNYc9Va8M&X-Amz-Signature=ecc106e0a89fbd6440d9a1635b671ecdc0d45d88904e27d10aa5467ab6b19b8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
