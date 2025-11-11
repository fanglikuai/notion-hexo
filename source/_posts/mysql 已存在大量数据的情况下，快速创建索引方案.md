---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLNFM7Y4%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQCqvorJOwlMRh6A%2FZjrEcVe1YToZQkVbBxUe4ubV3OkWQIhAKawUggLRx72WVOyX07Dc2yaK8gf2LUm7eOgoMLLjxzNKv8DCB8QABoMNjM3NDIzMTgzODA1IgxzNzcfZf6cEVA%2FInAq3AN51eQPZ5ZlV7Ynd2JtFEgaWvxWTtZ8j771rLi1R%2FsEEf119O1zcyOYZ4SxHumkQeiF1Us6MMa%2BaiIh%2Flign3ovdOxXSFvYRvT8CdvT79ejHxfcvEhfv%2Bk7Eiq5U1U8YcB9CrlEmh9MhwXYBJKT7sD3%2BMgvyI%2FAFG48a%2FyQL%2FLAeVizoQ3YFk4RC3G58yy8MUW6eP05nO0Szp0OTwGPUXQ%2FU9DcuO7qnZSB9fbDz%2BcqzIAvhDv6OYLVHs6D%2Fp3pzmR3HbJ%2BZoRncbEYquZjjLJUyBWcTqJfSH3feUZGkWNXwAcx3baTri7TM6YvBelIlEg7x9MlqxZ9PuuYP0I5g1BH%2FCLDa3CGah%2Fh5pVczA8eBOgoDyaps6xuuHvDYgF7s1PxTj2VQGAvjGhZXWSZymvtBuahCxpwYhhyeFlPnS5vpmLkP7V1iAL3Ex6Nfdt61%2F%2F89CD58vKPRAphpdEkFx6r9dEoxWj2tMc3f%2BhJouw0qrHtgOZT2wFleQjQnRMoGstmvrx%2FcLxqPJAqJwzofND%2B%2Bo%2BpFlyzJ%2Fr5dDdPx4PJ%2BOUyZ%2FMsytepCSEaWT8%2BdDGLn%2Fgxxq0dUsfEXbzY5ZU%2BSfdWOhcAuDGPbRdF%2BpTv2c5KUAxiewc%2F5eF3BjCD8szIBjqkAdlyKnKoPGF19zpUqXzZrDYHuC762Y8YfwWFKFWoWmZr05ixFi0GZKR%2FASUcCDHYh2F11Ze2odMKkZqC8LNGqakIfn8DDDVvaD06n0DEVi9PtHuJ6rXwsREqcMuhAvVcjsZc%2FRN9GH%2FeAhYR0%2BY%2FjJ0rijhIaQkaKOak6kP3SuordpzTL%2BBrkJfZQFUHTVgS5lPedomz8tZZNH%2BqHhJpKPZswJIY&X-Amz-Signature=7a1c44e2c9f5b994bdb95b2a9f2bec6644fd38ed57914208fb9c4f239ea612b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

