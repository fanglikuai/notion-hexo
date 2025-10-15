---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGDZ4W3M%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDO9rDUL4N1AhBbnbNikk3lL29ZS3p86UDxHgomhoOMuAiEAgLjvrl1q0J6npNsvSVXytj4uxT5RAusmJFSF9DumMLQq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDF%2FGfhPYVprfvHwplircA98Ry%2F4Mur2Bhz2Ehro9heoht2urMltYexJ1H%2BS%2BWtFDShPybcCwCcmNyz248KZ%2B3ZTlyJ6pJ83XxY4YND6XvKPPKGidzb1mvH7u7xmTtKyU6Y2KcVsXsDtz5bGawNVZRfKm%2BkRfT9YPqbJYITH%2FkL4pXfkz%2FSvN1jxlehG77LxW9mjyWvXzwpemypSEycEJXOCFBss%2BKO%2B3cVimY64cRQ5SsvMnswECxgQcI%2FfCqDlIbAYhg8GDo0R4ezMrzD4kWq7KohTXQEEQQErICrzFl1T7fjYjZo09RU%2FfLEBSWwcaKfls0Yhw6fenFOj3irxytctH0%2BCdRxE26sP95Rr5dAMv7iW9Of6Yo7Cl0KltpOTIgoq99%2BF4Q3sRYSp25PyJgdlc%2BnZEMf1IySqu2%2FiOO1FLaKFXRJXVOuyn%2FZWLLR7pKllKFFO1q0i59XIEO%2Bv2ePO9cehPaWgQ9BEzeC3zmtolqiTiD%2FJ9MzGFGKYE%2FVe%2BGAMFerLmer4Ty7xRHp8eDuGNFiEPBgfyKRa1wd8zxYoZBkba1ypbTRBRvolhlcpU67stl%2Bweqb36gNtuoeEdxaoi1557BGhPCF99Ga1PJHNS2yHGqVi836M6PQvaoVlaC1M4DwA6D%2FBnkTVYMOiYvscGOqUBTjDfl7YPYIPZxwDuKDL%2BAZgUsHjqSeWMUNmDbwHQVTob9t9HpkIpkpzevjqScGQPr3s8qiZ%2FOfMszg8SMVSP6vH570hgQIizeIb%2B7nf9K5JyNFBE2dpT%2BZNEaPDdxC9eqgIA3e0ktWwruUupskmK0JdLJdC%2BJ%2FEaIZv5hQWAKyXbdCpJZNr1L91w8o%2BXfd9t9gqMy2kd49ohc3w4pmWQxSlo3pHb&X-Amz-Signature=85b81266de1b0c9d288eb53125b9c8a5320e005c8e1d4a1b81e1c1672312355f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

