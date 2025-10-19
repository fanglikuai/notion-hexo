---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLFUCF7D%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJHMEUCIArjIJBXgNl6IsUmBHkLaEuZJquVuhvF9uIWiYkldi5jAiEAvn4Ecpbg%2F99YWLruOOHr0%2BCeHhpMdzksRpA4U47ALWIqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFWn2e%2FDk6UIwZXhWircA%2FjB4bH3GgzwClnGb%2Ff8J3LyDL04n%2BB%2FqebR5YN5NEhQlDD4Wscb9Fvy%2Ft9LuKjpmK9PPtoGgEwThhgBp2JGM0IuJoM7JSrcBXsDqsqWKkAPGoi%2F33beKJGRPQPL2Kz7eRpCXukB9hH3lfbH0ZxTmJqL1CrJOwbAwXJLkdndSNPWr6asd8a0RK2jOrctus2Nd1%2BaAeiwTh33v2OBLPNnDtMBby7r2uxNHpI5Jv9dLMJfOSiPcTBjN9Pvv2h0twqdvWCJ9mRXzENUm64ExfAULyDUYl0hOvrZ%2BLsnNBmfn2ZR2f0EcDfqZvpDrYUFuR5vnGHo3II50%2BqBCzi3551ty3e07ex0SucKdA2YOAckxPftFQSoABBzbSQaslEx9MFcjVbawYtWVuLq39FGVG2gsd2cb9yvlneFIx1ESrURSwmsLIBFtxSUD5JF04oO3rduk0G0jWlnP2Wv5Ow5jn9xaVjjrp1oIhcCNEhidSDgZCLjCz0WkQ6WlZIKpnTehPk31u8ED6Hn73%2Fp%2B61N%2FRJyk3IWCI7uDw8HkwfwvBRqdhw0M74I6aCVDHMooSJoTh4pGrvoT5JEPib57tLzRHoGqo0XjmAsAEBisDlGNpn27QWOzEmuMuFhE%2F5CBokOMIHX1McGOqUBs8s3OPXQ8e3nDmhcOcOD3vdr6xJ6Ch%2F3AwqSuiKvMNNlb6dp7VhqzOc8SyRsdXa7exR5gHQrFRQy2HSCONVnbn2XosiUOPJD9v6JH4IsQ3KZYE90%2B6e7aTuPuX8rkZUBscC%2FzbuS7OYodByLexc1XogDCNWZfBywBmEqAPBTxm4WxQOCBvKKeoAJITV6IfVq7DIUSuviGgIlHx9TCMVZzMVNxxfQ&X-Amz-Signature=d173d72e234f9cdf67af294648fea9d4c9ceb3193f89268a3b0d99228b949a4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

