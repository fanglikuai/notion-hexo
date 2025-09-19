---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHIS3KNH%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIDiMydSiO1FjKUUfGZdNoJrWxTFMADfjx86pHO4QUjd5AiEAvGuoGKFTJeH76MJfsXIPNrPrJ90gRVQKOCeIWiCSNTkqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN4MXziLHffyFmjkRCrcAykZHzspC03DTISpOHiIYsFb1hgA%2FA7hZe%2FCtr0I6RxmxRs0ZIs9QKi%2Ff63ghSnyUmcc8877gL0ETUmN8p82xiRGkafzSnn2UYucBEkZKu2rSFy9U4tMCOLzzOB7HBQjfGLLpe3EcbnsJ4IUt%2FeEZ4Iuy7qr3QpVRRmxY%2B2t0V8GIIEozNwrbyVuJgRlS81eVCFvAvTBkf1HWvenvHt0sj3%2Byl8uOqLNAGu%2FqnUu8z0afBKUz7jWudRyRL7gcy6lfsu3y457K5aFuxRQEDUHpILqOPY3A%2FDwMZjOHQpUArUusm9Vlk9%2Bu5SoRnWlD9BhRwLk%2FilbjIW1yUeXlnbbh%2Bz6iea5MzMBHqt0flP5xduxGLFIXwzvk2l94Ng6fPa%2F4BSn6hp%2FrlSY%2BaW1vCk8SQQqy69%2BWf%2FumCe09YQvu2cskS%2FT3y%2FCoZ6Ez3TKduKOOFQwJ5FZtz0r3qDz6Idb2O3fDyk7HKUNepQ1u2ChQjYt2D0%2B0382ZT%2F1Elx4wifT6NjFqo0GyZVtRwwntXJfbsqAfsGyTqGBC6LfTddx2CuM3yl0RihXomnPefAY1izpLwvkuOGBIxdzv8htdMm8oactVP0PkDnVJ%2BZT6l4KPvxAWx%2BRbf9v1PIyMmnoMLidssYGOqUBRSHi9GWKvpFuwO4mhbhCAKlh2yRndyqmXLd2VP1XWWvmbM2I2wMjWBjliWxl5Jptex8ha0bKbmd2BVX%2FFFL1syXlyd1tPI7Z4Wd86zOWzW7li1yHpuDP0wg%2FOmvbCRgFHIAsdO%2BHwk%2BXKxJn7bgtz4PuusAQyVDABSi9m%2FoL2HDht7xJfQqEGaebGzjm9zXi0SS04RBUYw1RjljkFP%2FUuElmZxQn&X-Amz-Signature=da06695b86d76834d66cf69e1c442983fb4016491e315af9d86dd72f8ea9e6a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

