---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBLEU2QE%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6AE70oyVncerAJFH1A%2BhrCSay8tdT5bDm7wD1YclzEQIgXTfw%2BiTYKLOrlUZTZ4Ml9WeCLllIetV3kWnQgDGlQToqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMEIkycKS1xQRrJp%2ByrcA5dBxJfnf4TM1ansLkjKgOTWwvZ7bpUHaAfMf7gHoElmouFI9tZSkvg3849%2FvJoibfvWeVcCwUzeLLgZ6W6JSfwLtrz62h%2FxEpBVJJ%2Fvz7VF0NdoP3b0%2BqILYdHyOlw%2BNYg9FCpQGIr0GQFuSPNGLJnDFT8H0f%2FN0aFKNtU9oJzxKAHiE0AYNDUBr4spaUy4mEzNc66UAOFwsAVb6VU17axaUG62Y2bWKmjX2PLxolC0Yybi9HEpyYgFLm0sXpk2i3CjA5ftiYKpntVwjt%2Fm0pPSoHoCQapkSQkkr6m%2Bo1kWtVUU6rbnEHYyj7obDuRCnQY4JnliULU1n%2BtpaQxT11fyMy0%2BRG%2FgYlIFYD1xih5CkOQ7ZqzG7dEv%2F4stoV4V2EAB%2FsPlfDr7ZEIhK2bm3zZ6M7fGjzXyZusDwcpjL%2FwSSP2V2dOlhM4KipMBUAbWo73tfJOZZ40H%2FB%2FGWCMkcbqT%2F2jbWc%2BM72b%2BbwemNT4U2KBYtqghWHd8mz1Y%2F%2FhkKL0xZeBrsRbqkBVYqsLClqdSFGPpKFfcmks51d71MGO5Ea9SorZ4SMMiBHidiELwuEYtIgrSQ1q1uoy2%2BaXhu%2F7TaLqrfrLSw3EMvokVkA0RPMQzlapPPBUyhrpkMJOiockGOqUBil6O8OL9I0IprE0CB4VEnct52raWez1Xq%2BOVzLEgze2ii87CpCD1tH18%2B%2F1%2F0u%2BWQzF5%2BualuF6uB2wPrc8u1ioZtRaFldmRamhEzYLxFGjHDC0%2F%2FPrddkOd0HnZy8%2FwJkPaXd5N75PmxviJdacHr%2B3sNv6YXRgVydAnB8VcIzNTJ4cvlyHoDcbYc6UZgSUndlhD9W%2Fk9M78H4lLjNmfLn6p8FYR&X-Amz-Signature=baaaf299c2204f829be84a6a2e993acc1235d7f3e329777d399627c8f79ff325&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
