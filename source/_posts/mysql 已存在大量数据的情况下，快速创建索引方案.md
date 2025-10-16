---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZZPTKGLK%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG7AYbbNFKU5foKj82GNW7TovTKUoK2dMnBh68479EFEAiBMF9jwZbfOG2Ohdljn2v89UcKUltg67BvjSrsHcp%2FJFiqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMolD%2BKdyAp7NTb4TEKtwDhpjRdWimelQyzP0mBVt6JekfFmILPVCYin9Hfb1oYKsH5xFvQSUiRsqOAKlPzHdoNaG%2BcnxxoGLhFU3FW6FI2ZDzGDGfhtLUx82vssswtbtpkvJc2xKKq6GBt36xa%2FD3oIe0w7Q%2BibpXaOYza%2FBGROc9CZEJd2kUEWStl5tadcbhLLpB0o6len%2BdZMbCkU3O47HQ%2BbqL%2BeAehV%2B22HyftJxdzJ%2F390pUTRDFeW40BjOX3k95Zzb8adk8LPgLb%2FI8IIC5Nvg4no2z%2FIGHPAGxKX%2B%2BA3XmFrgSmJE6F7%2F%2FpcWqptVtnvCZlyGifhbn1a9HgF3H%2BMMuJyd3TJ2TirUrtIg5n8U4x%2BgjEvtzKKBz2SfnXB6a9MJOIfvP6s8obOqKaSfBQ0xp1CGEPN2asVaEjH%2BNgHOcj3kPBgs3vl%2BQ7KlTQP%2BNT%2BuXVFxIoFblAsDpzFwUuhTsLjVPYUhDJ3G9ONipYOC2ZNimEhoRiwyYvlv%2BxoBO0sQVgb77gGIuR50BIW3Tu%2BgOJ3b6UUeXdhvR7lGNcJ9qdWJxrj4ah5EodYXEzXE13Yf8wY1g9MXETw%2Fwtp6%2BV%2FGxudrdbgi6qwF%2FMIsKurKytYFruA4%2FMEDLlUl2%2FmJ3yvT6y7zfWhEwm7jFxwY6pgGBzwTz%2BE5WUPsXRbWXrlLByCNTM8cqpvP1mFKbpSR077Or%2BSA%2B2Pck1Hx4aprwgNCBYHRI%2FEd%2Fq638UaJRdpKanFf31cystd%2FdABFw6w5aiHJIms7bkMVnpPy%2Bnox6eJ1VUGkEt8sp%2BagtUurt5BuRjsr%2F3ZC3PnfzC23LduEDadB6WChKvwrAZDTqpivlsTNpDeAp5%2BZsPT5RF%2FgzNoZTRqL2KI4a&X-Amz-Signature=be9443d76847b9613cc9ec48f4c498d6529b7bebb42786ab4e0282de9506e07d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

