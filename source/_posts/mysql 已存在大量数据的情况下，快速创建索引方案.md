---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TEKPSVJ4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T080041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEji3l%2BlpL%2BXgtBO1vtn%2Fn4bhZCg4q77GSe1%2Ba5uLIDSAiAinpFiLUJJ3kY82KZy4EubyB1KuNHXSxZtCeBBjk6%2BYiqIBAio%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbXMi0I%2B6JK80p6EpKtwD4D6if%2B07bsrERf7PTWmUXEDVJ12vICyub5lJKCUCtB4ZKw240Sg4hsBmT4Uvk2Fajez8UWUf8knrmXJKxkIqnt%2BMQsKLndu4%2F%2FhwHpknt9RKpRgQ7Z0pd88FSwhpiDEXQtUtCBies1PGtqRXaiX42O1GPwcB2ABmtdiIRKSlMg7fqXRt7EXYV4SdQL95H%2BaF91J9QzAZTyx%2BcBf9omizSzWB15N%2BZBDBhPtvEFAZlV1DXZLe1ZPxpFw9IrxCsN42v2PpKVY7WJ3Q0pzPnC0yJWzYAEMLQ0CsW2Y023Ib18njZTHtJmC3Ye0X%2B1z7jBBLWeiLSJhemsSX9685i%2FIPR1TUvOTJkFcfJM4zQM5WMHeI%2FJdCOTVdasUVIw4DMb7Eticn8%2F40I%2FwlTCs6ytMXBMw3KknI%2FqZTqOtPYBOGEc80jQsetWkoRDDLrzkhra%2FRg3INznx5cxW9zExmrkjnytN%2BDcYD%2BkWbnmhbXwx8COlHDRCayxKJJd28FTu3MI7hY0Bpxqe8rtISv8jpaUtrJ16JQHpUk7Wf3ChdzzZ%2B1wEAkWpCCDLghoxRyByGZuMAaVA8PDYGYTqHPw5FpxT%2F27tMKfqUHn0QTwsH1YGZ1YGGPgjnzMwv5jBOQP8whJbryAY6pgFWUT7BmMKJ05BxjnqIsOtIUlJUCfG3fQYt9%2BGZl0%2Bb2%2BGSM9qtv5omy%2FMA3LxWXfmAno4R0DqWNOilLYAy3gPLC9SL78vSEMmLLJymEv0inT5a58cV4utoup1BiYztWh3mn%2BtVgBi93rtD6Zr%2BUuVlbHyAnXigEtIlP10U04CjPSlYCe4nVOYK01WNecx1LM22TSFuXa0MjEzKsiu%2FPxiJWX7HLAHa&X-Amz-Signature=23059e86ba5048f77592216e189b3e934b95bef506005af8fa927f4566591f9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

