---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653E7I6CA%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1PsLzrufQ9Z%2FDTHyJopIFOCm3GfD5SzLa9zeUJ8P3TgIhAOM9zYZxu7ijjMacalftN7lhy3bIZkp1FVMpln9tJa1SKv8DCEIQABoMNjM3NDIzMTgzODA1IgwFuHThthz4cSF7C9cq3AOTORvbePFL%2FnCuS0IalEyzXgLTsriO1UNYlq3N4eNu6r2EQex36ehP4yWbUZ%2BM1BH7TOlSWQU%2Fj9Gg2cwxTMqJp6Et8sdjhprICiO605kpsbAefUHMwWSVSwfPSlNIRR5sJnckKx5AblxvZy1ZTsoo%2Bb2fXGYrJav6YjPeHrDj8gTwW2nCs09zzJHUsaWsDT0uRd6%2FkZZZ3SnnSVrGIHzfAQk%2B8QxdH2%2F%2BnJUEqzDpjYpCvYabpgm%2FSemNsUVFXBVWzKEZIXvdllBhlEnYmT%2BlQSIUZEGFRgLuKVdaq60lzflON6ve2H8T7Q7jrRJNRueheKwNzMMml7OsAxtyN7AcL2aD9oKOmVkQ64Hllvob%2BQOQ79eflwLwolo5Eqf5fcGa2nO7Tz25a2E8dANQCHyw%2BnUQxK8k1GoDleklNkj%2BLIRy%2FuT%2B988AY%2BF9OKbMqF7B9zeX70noIKdc8MsneeLONzvC4Lk60eWw6zzxQV6SX5HY4%2BLqzxUHbgOZRerNVJSaf6CaHxUOA4qA%2FI3HbEQB3piw3TLlxOJtXY7asO6k7aZ%2FAdd6wME0SBPaI0FGXtqw3H%2Fhl7lc%2BnksOWRCN91eMJKq4zRxeze3QAP2uCEktTlVBT3dj0wM2sAdIzDGgrPHBjqkAR7Eg0hTVwsUC3tOE2mx7mTEobaQAjgtNf2NVXBEE7QExA3zQxOKyOEuJ3CZU%2Bmc9JSqYDWng5AVfrUbo%2BDgFipZwhpiyIqaXWGGmHB3SACuuM1Dg0h6p%2BOgaggiTm1TnTV95Dt2tq9%2BSVs%2BSl5xUXzTyzYiWH43Qy%2B6FhPVsNpkVg3%2BerblNMs9TMVXQDXgtO8esh4d664I77Gc7v8BpvA8hGLs&X-Amz-Signature=79d5e71f329bd5b5de33188debe31e47ce1cfc51399b668d46a6b1c57e6eaaf9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

