---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S5A2QUIG%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIDg7yIcD0EvZH8iRKUNgaJZgvo6RNnyzMKo7Ae%2FHYjXiAiB7CL2AFgUCq4QD7VzXG34XibnFdDpmQ4K2bln%2FJSNGVCqIBAjR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUt%2F4GYfv2YSOP7zEKtwD%2FjojGXMkIS5b7ao9eD9rG%2BcXAZR0MmloO%2F%2Fxz3vr8Q7tmjSDYEocWpSzpJDaeLrzIaqpuVwGNPG43sR4lVgyynWBAs3qgSlhMmMJktgBKCCe5JcHOQepHdmvUMS5GZLgfYPD1w9OH9N1JoqrzXhD1rykZfd60Ibvz75PD839i8m3R%2BH2E%2Fqd2zOcvrINCpPl4FVfb1u%2FgUnIULiqlOTP3RZa8ZYKPxoqwTE6IbMZUSGPumdHjlv2wicj99UHfPzWZ63TouDJLuRDzH9Oa0PvaCJc03H2wELuLCsTpPM3eewZq1n61mrm4PWn4WyUpKO6wzFgHvT1wDDf%2BmXsOv%2B1YW8bT5nceVzKoP1EBa5phb1RYH5HRgq6i%2BpO5jCpsku7Vke4E31Ke0OsfD9McF7yxYbVMcCwGiPQMi%2FUZ1gsIqHSDS1g7ob9aVbeJV73GmqtnZHqs7v5%2B0cs5AFr0VYdNSmjapwXNwWldEpT3wUpJnpvnhA3skXIRS0EddUo7FWXRkNYJSqgEAtDWH%2F4qAeEx0HgmaPNyCOpqB6iONiSGokMiu%2Frg9aD2Xrp%2FDG5x8%2FuaRiHTNwZn%2FMP1oSr9%2BLNuvQRSY7Akw1Q0bzGLRmAEAOuCwtEpyBr3bucjmIw7%2B67yAY6pgGo%2BA%2BC2CntOCdxw5TdC9UWJu1pAKeN6w0tAf%2Fgx%2FsUGD7B57pms8R%2BlJFzGdATvJzj58siIKi%2BJWe%2FfL9%2FGdvqBJeboD9Q%2BpMWvaERqarFmU%2FXfDW2OJVp%2FTUIa4Xk8QW4ezghLptbYH%2Fh3rKP0tiBvKRwhZ%2BaZNVWGfXP8M0m48Wf1Fp9sStZAXZLjFU%2FNesx2107MFvFCQ3OZYfz0wTjJGDHROwE&X-Amz-Signature=94e82d8218bc06cc8491100a6eb9ada5a5f2008e2f8511113886291a3803833b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

