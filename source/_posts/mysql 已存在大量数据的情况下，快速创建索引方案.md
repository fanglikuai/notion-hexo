---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QEJGWMH6%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIQC4u7gnK%2FF3%2FjS6pqFV2SL%2F76GlNo7F6oexMwI3FEjTVgIgNebFh4COiSFZhorftJ0PYS1sbYhzUuh8HFBrV9pC8NAqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBsnE4FMjWnQKC3%2BOyrcAx1sNWwbKg2m%2BqwT8gVI8euQWP2o4fsQ5hhDJH6XdZ4P9BHmQOmcHnwm0lAobg6qcMz0wk8Vws%2BcdlQ9Lbxk5ODY6dl16wyZSCyasl28wMwDyMXpKrdmxIz3XQdPdRde%2FmuWLo4iCIVpk3xZVa6yssHm%2BpXzraUyPwj1tDUl2lLC1S1sPwHM7NfaFSHOUBP61pcM6BxVQqn27vELdSkHlTnJckI54ig9tva1Z1KMvqiRuUrBakkZXSvTC6Qa0JTBI4AlMQ022Z35mHj44cDXLYETMBENRK3uz2TNRhTu1MluJggK1I9WakMupdP3QypP%2BZiKC13gO1GzmLewTwW%2F%2Be2kgibPrAiFWqIxAN263EjVVgaOXhh4Nruy9HcTSDafxxyS309jR7QtuJdz%2BmiGPRrq6eE7UEDPYrZgoLx%2FPUBAWc3TW3eUrIv39R4OKsoT316u9kwn6MrKkN9Lqer%2Bz5tK1niqxFR1Rbbj2jLqVtuMT0W%2FchuGo1w8s9mE7Xr0Z4fPN96BSxZMIAvfMRBDqWF%2B5KA8tdtu%2BUWSeV7s6sNo6CrziRHtEP9WUoDDo0qn5H1m5VbZzM2gmPZkpFTJAis%2Fon9IBsL5MwfDcgp2219RHcYVhcq43WdnMqDGMOK09sgGOqUBhXjC7W5wMoF43CG72buiMob7Vbbs3bDAkx%2F52wSWOBOB5HTnJIOTR2WUJHmq4cGXFyrMhmpkyPa0tKioQ05H2mA7XS1e%2BFS89Wa1n%2FksmbrOzr3ih%2FGbxW8XekJgvka1nLardoSKGnlBymsBdyzQeHTOSmRi7Ar9SNjHVPYb7eHmTSTOdhffboEJBSrBiGG9HEK5LfhPLYgp0JM0ShBbY%2FRqEAJV&X-Amz-Signature=e4bf679483c82fa2674c7451b6d1eb7d6351644ec6af77161142a05c9e9d8098&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

