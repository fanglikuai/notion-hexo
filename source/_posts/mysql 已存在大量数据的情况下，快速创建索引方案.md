---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGZTP2Z7%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T220136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAbHKxsodESw4xdyahhnTlpSuB%2B1Qlw9zMnmfBQrWnJ6AiBSzjlcXLoAvHDJJBjyrpPwiC%2F86CO27ZkJl%2BOs8%2F2bpyqIBAiu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMuRk9IIqIX0OzfNNSKtwDXD7KbGpPbcSCdMvL7MgEzt%2FSkvQ%2B5DmnhQBqOJIMA4gSgUTaDGmn7IkXrFiNEiPxGBCxiktZsSgikHiTcZRNlKK8P7SJ58nbiuVgPG6xn92w5tcvlK33r729KE3E7gh9AyccRPp31ObaLuuldCyBvn7oT18QiIvUu3upTKFILC0ZGdfHG7Lvr3m4iKTjmiiBwF0NsvxL1RSgoXfGJ5m6NVu1Lrq0E%2FZouiat8MMoJ8oEn%2FIuOPWh94oUsj18YGUo7yUke0BxnWLZOnHOlWSUYXU%2FQmqs00xpPIi%2BjE%2FoCXVo%2FPy%2Fr4gqhyDZCtpJpEiZBB1WngM%2B3c6Qsw2XPDWk%2Fu4ge5SDDRI1WCUt%2F04icc3raKyyTrAx%2BVUuz16DnA%2BI4GtHkzNpqSG8%2FKWMf%2FwGEX9e%2FBsZ16bgbfYnqRmRJUR25Clcjk23nv%2F8QjbGpnnbOcj9gxAG0oIhJ4XhndXZCLJt8JX%2FcpZ5D5RV8dUw2GZMjCDvyN5iic24Oc81NqQPjVAchYN9kQCFB4ZxW2KjeX0OFZHyojRcK6lWzPMewsGGcXMttE0xGHDTZ4gRtS4kck8iRcin0U1sNjiC0A7PFHQB%2FL0zLdwAzlHjIUTPmi%2BjJr0gGROv0CwrepMwjLv%2FxwY6pgHTsdNQ17im8Ofu0mZDbxc%2FPPUdTXitmMKfjucEJ7XL49ojn%2FfgzYrgUbQBsetovII4PLoAavzQMpw3mloFPkn1FfkDJ1E3jcP7fKcNwsZm65tF6RlnhGGiQJV4a4yzoGEeicjqTlqnxOdQIwdfLRIX8Wg0Azyw1ePMXLj59ADWrDXmRjOTH2GLnbRYmpXhbBkqVML%2B5e66rK8KZAfAM02kRwyZ3W8K&X-Amz-Signature=ddbe8de5d2f2d5d2ff7dc9b412d968c7a40cc004285601ebc1da0c16fb142e23&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

