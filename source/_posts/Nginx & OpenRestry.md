---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QP5N6TKB%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T010049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqRlBmgg7c%2BhN%2BJJ0Qmn%2BXMQA9mehDi3UQa4R%2Bvkg8wQIgUyAr5skWULV%2BrqiKXCUNBDofCRGB1BgysEgsg3UuL48q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDGqv7vN37wnOFUbfnSrcA7g%2BSlJcILr1Mx9mTXPbrJ0CAttaO7yN%2FX%2BC407CAh12qTnG1LlLNYlVPEZsY0u8wSJdYboYa657CpdLO6QZJUiu3bpjfBlnbUSQuSdx5SYNZuwPaK%2FRyFKoIYFL82voE%2Bt4Wn0kLPUbjOGbXNkt%2FrHCLYBHbOM5HMGAfT%2B0uhzab6p751hzs6LlTGJHUeayKXFrEZldZdtCd6wvl3UPTQnlv01KwZ8lt7DVKAz9f9N46mFMzF0LFijxjK30WYsjndUEfuhX4EiiTHHalAWlRio2HfJL%2BAOzYwweq9UgBk5gh4X004W%2BrtcKgiRztj9%2FvTMMtNJp6m3MSCvWcCqyjREvBpPdvf17rhxs%2Bc793Hwf5ejTgMWR1gWLrztDQC%2Byva5ZryVFsnA7mg%2BH2hOgAy26kh0Lzl8R%2F%2BxeuXuADR1JeYm3GZatqezeGaSWhYNh3s88TQJpqKPzCiGpfAUjcPUUQfMAyR0B1%2FqXmuz1RugcvNhDeYUNJbmxj1OXetzBJzuRaOOKf50j%2Fq%2BWGcQKu0KJwUV4fQxuv8RRvDJTjEk2R7%2F9bk5c6CLKmJnz8x86rHTLFKesjZPmpGtirN73L0VjersZV9MXKO9Y0Ef1fCjy0B5%2F%2F37uMPQWZkWgMOb75ccGOqUBeMHmUS93RCL7cnJNqxt5%2BaKLfWwLZyJAlXablxUP86GiBKmRAakoRdQDdPZvzNaxT168%2FZ1brTioGQUgWkkNEfrpsQvhX0PceB2w0lAU2Y24zxHHm2ype5juX5efBayagpngm2nJ9gu7BafpTvjNDlafzXVVGuPbnaHhrw2l%2BLbmvQ3RvXic94odJOzBe3Fl%2FZ5WZr8jW%2BH%2FNK5nqlJqlCZGprWJ&X-Amz-Signature=c3f304b79960a5db1758c0c35edb5921cfca4f374ff6acf566a2f315707a81bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
