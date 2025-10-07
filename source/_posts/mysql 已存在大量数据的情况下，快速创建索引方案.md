---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCPIHHDK%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJHMEUCIF2amej4iy7pJBLXqLrVi8xHwdfr2VXgbSfpkU9EgoEWAiEAkHocBYp331yOkel01QVkaRO5KPAgAEiQVJf4COWgVyQqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BJHbJbAKaHEzmdtyrcA%2B9NfcmwjcXbAz%2BMhSFJvCBcaYf7TUU57Iqa6jI81YuHNTwdjcipdVJo071wf%2FZ%2F6ANuP2DTjXjNfT6LIP8c9ay9d5iUKOCN3c00uLl49rn1uz4r%2BFGKp7VOsa4Wi%2FUKUMINzjkeJrvVKW9lV2xvt9jO3x5RMIRi3wJrvzukd33I69gIup9D2AvIZi0HagcxGvNlchZLVnS7AFwz7RWb6pOPqsf%2BfK9BdZdQ7l8641WFyEnhab0TWTQiPB1ZD2RMtKJVVS74GeDkZqDpVn%2FoK46LXXGfgrkwK%2B2zWNficuQIEY0gO1LL8I7%2F%2B0GJEU5o4wnGBAkwjdv79e8TRB2CPViIebtnvCrq7ORMv5BoHo8A%2Bwp2tOj%2BO8J1ACoxwbFmMSxFg3J5%2FFYOYTzAbM9FV4cf%2FBSwg58588tko08Xvkkcv3xrScRfE%2FXS8GHNdMCR3Q3KRc%2FvoCI2pl5cBF2Sq9CkV4R0EMda8bF%2F1i23asletL9CiBDKRpG%2FdQ4NhQsd6bN%2BqgaPF5Mt2WHu8jCFcla9qoMO5up7so%2BdLeGl75XXPprKy958uGhKT4OyKH1Rsvt7fltYrjrzoWOyFDWuiDLEtiOrT73rsQF%2BmRnqHIQhdlecNRgCn9OMkzWOMOyvlscGOqUBUcKB3yNcRJbBCTKcwHBHI9fkq70ks4iTXrPuxNFFqqNJpYdpENlNGhzRPf01%2FbkClV98to3Yrq6xOgWTq1RYJZgvcuNKL4tcOdkxvnj0VXojnAZ%2FWKlCupXGKWrDRobvd7NqNZHBZJPvVQOWwG7c3htnC8MbyMFnpJpBgs46ykMTO5Ny2gkNO1IYArLIJ%2FiUi%2FOp18mx1KLB8igTMpIwDw7dS5bz&X-Amz-Signature=60f4dcbd68fdeccfc2d5d72f252e5d2a5354aa5113302d4a2d6e5e44ee41660f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

