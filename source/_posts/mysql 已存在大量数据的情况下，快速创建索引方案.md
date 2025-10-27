---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QJ43LJH%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEsaw9FOXd7mvI%2B2H0wL7bW0HIeydzzR6ztSlNxmUzDxAiB7E5y%2Bnbr7f733Dzw4Mkd7kW9Ser6RJZbhtJSWfHCjoyqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIML3ZQL2AyMnW2NJ4ZKtwDkjLf69FxZ2yFsKqCbBll7Pa8AoF6AQUJhqL9RBjbAV621AwcbZAAZa7Hby%2Fk%2BrL%2FwRtX0pde2gZWq6GtpIFqQ%2FIo8Edm8mgef3P4vibHDZqNXa6FaPunNAfANgTgZzY2UrjuMq3jrMMCrc3dUjvVDenvenc8fTHmk1bg8G0FQl8yY5MSMcn2t382EjTGTIKTR5ObD8bHoKPsvmLx8%2B52gXIq2%2B1JeApoQLXXRZFtWKtHF3RYj6x1zqZJNpwQlOfMknloafp6UitfqnHcdsigvLTesRVp48IkPyFhetmWR6dOsGpRMXfm16mlDF%2FE0DlBb8EU3D%2FjDayBQqHLicy3%2F2h0KaZWkvuCjOitxaq40fgQBXO92IR1WLoS9aGDLIDttFc%2FLnZ5rrlS5TWStnCJB2LDgg5Y%2F%2FX7LfcFLQr8GdvF1Xp82DtRLj5S4BF9oCp1C423VD%2B6NVbFS6Ba7OJal5xq2fxtFLR29NpEa%2B643w2aB1IdzC7VPdig%2BVagqLi28%2BnTuq9RK8ix%2BdBeHfIDF8%2FAHvAxtdagrGTUvSSVFWJknaTpnSSLwjnK9PY4ACXfhpR2U8BeHb2iKZcKuWsyDRg%2BIWCY2hRwA5hUqK17J%2BOVQHLYyD2asS95Hhswkb%2F9xwY6pgFCY%2BjbOQfHg4SC95AvgIgyvq1Jx1UtpixQH%2Bccya6OtGHkf9C2FJbefKJ6eGJjKRje24FTXhzYk%2FxuoZjOcoqIM%2B9I3PShULOaKc%2FNH%2FFJDHf8B9e2yo0GPu4zinEYH5MdP79d9Aj7f0%2BgkaKnAPDt%2Fr4bqF4Jj0CcQ0Vk%2BTfraIi5bI78ZFPhH8%2BtZDKq%2FeCwnqbNoXhfFSEaUwAjii8nsMAwdAXK&X-Amz-Signature=d7141ac8d42c89678cf330f03f0d47f325e2a3465fe865b532449a6533b32053&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

