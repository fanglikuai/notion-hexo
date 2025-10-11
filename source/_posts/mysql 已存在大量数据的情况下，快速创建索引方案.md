---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5VNICCG%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJIMEYCIQDm5AoEbVmSY2p%2BLBCyuMRkagf1HjQn0A%2BRhiGnYvGyFwIhANVbtb2pOXmyjOePlPFU%2Fkp0o4urALOj3lAycaHH5riIKogECP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwXR1yJXG8jf%2B3cjHwq3APmDdK06aYExddu1A05MGYFWWoCYsYGCXhE8UYrbkUsWmy8zYFLEOU%2FpFzrKTAB2vA%2FLlZFc4veUBSWNbro72vAmUGn%2FNQTCFBDfadyxUMGmPytcbrBXiqUshVYzAm%2FRHE%2F9b%2F4KcMq2fzhxa1U8huCNZ9mIHgrsDSuD3KteAzwu5yr%2FQJZt%2FJtqfKmjocbJG1mqwP9VXL6YY5NpDFb7cfx9xWEp9Ft2ytEX40F0f5EGKa%2BoohysYOkvdC2NBrqSFwlT8%2Bo6cXVtLyup700MYBH5JabOjr62%2Bn8R1TbbvujE1DJjIkA4GiOWhXkyk85dzP6EOE4ArOXy8k0TGvUTfmvaw1W4Jt3dAQMRkyMR27j9e5Pl6HjwPRPc69hhBmry4KouCheaU35Xm7xBXUpSoLLj%2F%2BOGVAqbQTGeEsjcjGFq50M7a6lZ5MshP%2FiErtoZgLzf4FUOmWTs36mob6gVoVoX1l05GM0%2F2FEe4HsUKeZliapFgq7Yv4iPV8rbz5LNHIdWh7Y9Ix7DLK7X6PQzWKki%2FOpb0ZoMyQ744rHFXitzF0n4wb%2FQf7J%2FG5lA996lZBHlfMI1r%2BQxXGm0hBwhK3tsIwY2RrXvXC97jj%2FXup3fD42OHyeGKnUqZOk8zD74qfHBjqkAfjTYRcl6T8eWlu9RdcqFjmoWa2yNrrrrTn1s9l3pafj0kZOYuCjaqJV9stwgx%2B%2FtFEUMO7KgkEvQuJN0RrA0aKOEoDi7i%2BjPbH8HHYt7mRND4q9BJ0TeLu0rRqCh38SgodaBp5%2BWp88SOWWFgB2%2BOpkCdCAiBH%2FI9mwaEYwVgEAFN46u%2BkCyHOiJAev6xHUuS2Ktcpn6nsLfT4eG89afQesjf%2BO&X-Amz-Signature=2c2732ea7cc8875a3527548c4e291ddd4b8b09ff9f83d5e60e41d377135554ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

