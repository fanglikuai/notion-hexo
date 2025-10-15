---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPUOV4D5%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC5xL6r6bYPCvAU5lK5a%2BnW79smD4JG4KMzmumhdkgV6AiB6eCRT1JaWj6MSiDj1vo9JKUTeP5%2BGECsSh6o7gWNtxSr%2FAwhpEAAaDDYzNzQyMzE4MzgwNSIMEqQtv8vsScVKYgHBKtwDGejDMgKuRZJU6caOtDtEcheY61csW8is86Ryh8TbkINDHbaxw%2FuP1UEvXbAl7XCIIuKoLpVRYk%2BQW%2BSf4fDBPrCaU8h2Q1ts4kqGV2cVsZPQ9kShkx%2B0f%2B7qiXlPodO2vbjXY18Nl8qlLtt%2Bw77wXVJYNi3dWTxTLJ2NVMzb5k9MgEda%2BXlm9VxhX0RFsFS86%2F%2Frr7jZGWs6vnPmL2yESTZBwxJFpRB6AxVhg0UNy4B7aiLoQ%2BO%2Bkeb%2FQDxeGHwz9%2FAYISPWk4loakzBADfEScGmaOP0xgEgAkREJnlbQ4VoNfdJUkiMydmTGflOJsnDi7hY9qE3VUb1YbmWJO55j5%2FWvy7eS9tnHod1l2zgDeXVRyWMoVOs1qNtmZ6h51Vibe4p4E5EduR7huCGZQ0xOO%2BI0bY2fMhu8zP25Y5DH29TCgeqH2LOBZBbqXI2qgBUddgv%2BYx7EvNvOKkJuF1Rs3SAM9wTEbEjirXn8KbWmmzUSYX%2BZ0wuTs3fIpS9N8sRmyw1qDM%2FHe0vMR3NyQrGYs2ClQPhlXdZk14swnzQMAnLW69lh9Btm%2BndphEmV0zCl5spnNbH%2BR377v9hJsj3sHb0Z4urPPgUhvJ%2B2TrrKZEGLtjvOBOqrLCkSnswi8S7xwY6pgHvCd9LCSk7Da6dpBZQxwzbF6QbIn1WQwsK21bFSqMTHxtDQwk9kSqN9h%2FCw6JPa%2F7vugcx%2FbGVYijqttLpz9rKTKxhAvje64HxWkwhvKU8qmrMoUhzzb3NS9%2B6rBz2wlC2XIKzWeTp01Cr0wrEGylZND1pfRo4WCjsuESo91VPR1d4Jy23g1sM0EvX6ccHGGXQShe5HgfI04mZX6gRHdSZhmoKpKxL&X-Amz-Signature=433da83baf2d542371db40b82316f0b0c7e102af07d45ae46dfdd37b3a350a19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

