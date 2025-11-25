---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662B76NLCW%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T000053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIExxAxg9RaqEmoWt1OBpXywNUcbid9a%2FeHfbj0BU%2FvaLAiBQRCTNvSpmGUBzdsoYsBF8QuAKXnwA8JNYWplKlkjmHSr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMX0dWJFVdCUQ%2FZA35KtwD0brZczQ3cn90yFlETucoPyguHJW%2BqeuDKABIJ4WXiqtrCltXbHvi5ireHv3LvivCLj%2FmKTtwulyujs514buz77snwU9WX8fd%2F9CqttwNtwJ2CuqlNO2ml4VFULl4YIvNHYkiLbobpTPMCRr3qEN3uy3W%2BvKjkcxVyWNGIQ%2B%2B%2FsEAn1ISatNNXjW9YO1pOJxb2H0xmxCni5dURtLv4ga7fnxUcXkEa6FOuteiQ8uTC8cEzbJObOu7CxaMjW%2BcRmqKacUBTOvFJpUWKxFSOn7%2FDMI27%2FykT%2Fz2RZd%2B3a57585hihvBvxqc1r0RyuqLEUcYJVMtg9e1YKV4yeumLLKnxLtL7XJnw2nnxWGb%2BCdj8Js2AkidamPmDh3%2FrmOIPVjduffGgPCDGK556V1gCNcKtbHdJZF7U3MtWgBRVZuXEzTLr4vkFpkuLMkobF%2BBA4y5P9fiQ7s1Y43vPuAPqPEJcDuINKL8FQmta%2FjaPUK83Fhh2rHqir5eBqlVqOyFvMk1AB4SIXlzs0J5%2B70Gs4FpgUkE7%2F7Lq3BTn1gnJ%2F5o8mL6m7zIpOVlKFoPVh%2FgtLYiKPzN26xmFQt1tjha%2FrxdzMmWE8Q8%2Fqswf9LBqOr7bl9w9BAW8%2FnVqQj7tfsw9daTyQY6pgFLDFreuYSSBupbEXjA%2B%2Fovlt%2FLvS%2FxvUiWCN01D5UGQK4bgNucpQLh8nQju802cmkQOfqDGKBEjCrmcAc%2FVlno6vYxlgcVWxS58KyaCpRgdZ29%2FBZYjF1u8EeiF8dOMdA3ZTs96FY08D4mqcfTZvQeYlZ3veVL6gA2Dyfen3CrSIrt%2FMN50CpR2w7xKbg2vbjEC8fHF1ggUF5z0%2Ft88b8uPM1ctqc%2F&X-Amz-Signature=28f5903891d199020c7b24c816a8e9825e66f36c113fc297030ab527e03efdf0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
