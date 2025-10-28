---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z42A7IYG%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDkceeB0jiBNTov12bqwQAFHrp5yzTk92VxoiMgX%2FhlgwIhAKywiVaGPFrYjP7mAZ95g2N%2FGo1ReIPcuUww5%2BooC%2B8TKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxHcnRe14rhx9kPoqYq3AOz0k3KIn%2Bav22auNeT6Z6eN4N0tW4C3IbGzWGbB17BmGbQDQfvNeZIeJcpsdpbnlH60nFozML30Ftgh6UsJyQt%2FbA5JHIUP1ZInwMWBty6GuBhFV4HlrMnWehm%2BZdSUab6x4lvltdzYtohR1znm6ndemgwPTV1W1%2BdSuT7ZRKSO0TucGMn7ZB8G9Li67pTKDhgZHy8l%2BBFlisqs2sh2qFWxotKKHYjkdVLoIBr2fzC0OfRUC5%2B751ziyvTaxTdNDExxsYwMGim1rbnMQ%2Fep%2FWlbd6XY0yOadFi4DWjrDcUsnwM5ZNCyMtHboRYREpf3%2BPbfIhj9S0RPLtBDk%2Bwmb88wsDh0Ju8MLwacjix87A7RQJNLPC6rA289lOIuA1uZ7Q%2FXeOMPyvsRikEo3mmP9VYJW0yMXe2SVy33dxOTVDmfBgUIHil%2FrPF4mCUVGy%2F%2BXfBuuVq9kI8A%2B7Q01PJcufyIwZvncZn%2BnjHcBeuo0LXovWPeneN0RwbIoIGEuZ21BMXVY%2B0o0HSAB8qDGfH3wj4HU%2B1%2BFnMbEYi28SG4TtQisg%2Fwuj%2F7HrQ0guWH5P%2BnBiLoCtMKDnQPG3r7whEsescpshWazat9fghRi8PQBj8fy8RoCnISeqlhpXxNjDmg4LIBjqkAWMgPqoBxkK47c3OV2i2dtRxGZA021Rnkgfupa7u4OiEPhFnrc1%2FAZS7amB9MKSkO578rPAAmQdE6qeiWYAiCiCKYT5U7mywQeKGw9lwu60NGCBc0Qn%2Bt%2F7G57k9GE3a%2FL04wBWeMWe83U4iAVDzJ%2B5Vw%2FBW69YD7SavpNZpH4TqctC9ySQZ2ixE4g2mXV0CymbNS2qYSuigtituKyKMkJQE4iW9&X-Amz-Signature=9574dcf125cfb3f4623c62db7239fa12cf5757a64cad99529d7e8909bbdee452&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
