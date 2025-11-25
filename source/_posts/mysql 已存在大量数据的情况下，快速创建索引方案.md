---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662A2PJABS%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3xE9fXcRHM7AKjT7m4MdYiGBcdGlixq9FsCdTpuvF%2FwIgBj%2BZ59UqtQ8MYANSaKlwXp%2B7XIXClM%2Fu2HTfp0t6Vk0q%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDEtwnwJ%2FK4RGuSYNHyrcA1lCXQPuptKlS79EZqDoHjod0X04ktwGnRDsFq8pftXj4BuyrzXY9%2BHdXoW5pGaKktzRJCTHvDZtw4szX5izaRDktqUUNIiAAQr1DBeWZU76ue09UtERVSVEhW6b%2FafPbowCLEyh2x9qJMNMHoX0udiIlehafS6VAb5jOG5CfDmEhIvuFH2bN%2FpKbCerbady3GcGZOQ%2FYZErEJvFRTZ4UoreCdpXP672VZ50gWmhbE923xYIGNKWppvvXJ6bZvrl0TNx9KPVAi%2FvvQ0Zmv%2BBGf9TiU6Wzgm3o%2Fcv0IACAsFaIzEWH6vETZ4z6sKDDa2zzr4LPNiCiGWt7rhXHE%2F6ajFL2%2BJMugGZA6Qh9zDcyIlzsfw82heL4pASQKfAdw9gRXdXVqZrvRRGXZ85lPZ2XfqhpRIPFBsOMuVCexxTJ3hv1Pw4ZLe4cTEmeTIjF32tP0thLsPBXpng%2FOZ0WmJgmhrWpW1fYmO83Qgr8KAkxwpfYadYQEt4UTLovAfniANh7Rme%2FQCLGgwUadG5N1R5mvwzwQ4jpWb9nwKNrdh0FJK4BMSsmM88ZAKeMp1X5YnTEgJN%2Bl2%2BwjEpsol2Acqc4GrAFDMTyz22hyUMJVb5HWDKZpvSt%2BKlycqe9wkdMKXWl8kGOqUBlTZzGVa8tFPA3Q6rX1eWimTbwD8gLAFeqxbS2TeufNvO9lZIipVtlWkXiHQXI8quxPr2sHCjiSKXYYO7s1T1CSiGKhEUkulYBdP%2F33kMCaMT1XasMQCMkce%2FwJpdAvhb1e9WzQDl88B1STNwZCCOZTbO435E%2BM0z8V8jVtA5yzR7dE2hcjaDepXWCJQtBSH8UHlEayyENWbtH1MIGdTQ6dWGuPey&X-Amz-Signature=b6809cd3778a806989bb60dab685f7f7b4aeb8d803210b8d0356edf065c1b72f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

