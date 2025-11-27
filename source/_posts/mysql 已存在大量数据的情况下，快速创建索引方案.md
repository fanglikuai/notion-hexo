---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVW22QKQ%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCcLbe0BocKFWiwi3ZM7jOtLf2yLp67Y6AQhBKNnV%2BFEAIhAKZSe50%2BEqW5Z5Kce9hGESJtWLeM2gmYT9lQ6p425yySKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy0W05hD71lzRG2mWUq3APYcO%2BK1IBNfCAwD9p%2BWy%2FH6mkxbwSn28ZcqJ0MC%2BwUBl76d574wkDBQ9LCkcR4ZLfdLOixFHFxkBmYqsaT5T2aQ0cWw7zNxnIgLvnvukpHefOLSDfREcB0k6Fd7DKV9BzxsxD%2FPdTwS6f94c66Jb3dwZ8bsTIKAjvMS7n2n930X%2F3PDYnePWuSDRUaz1h04kNoZEhAP5Zk8dn2a%2BT%2B0hWJ2WSIlkENBFfzkWxdvOhTAtXQChvzwSRxodszud%2F3YhHx3b5HupWKHAywTNGp2mOHb5DCxtZ5VJ1aoFAvC2NylYJy3VUsYoqVnQFkdDamJHgw7evk%2BG4Wc1lD0cxB6BqFvNOdAEZczXsPV9j0nDGNKe4G4k7gyDnxRt87IHYWCZ2QRkDf2MEyAhuY3Xc56atGSw3jEp9oE7kGHllhCGOubooEmcDqFAdD76fiyYV35jWba3fiPwCxjzslE987dnc9AB9sGaSDpUlGq73LBWxj5Ars1L%2BmErSci6EpHRQC5hwguSczdtg%2FH%2BSa89HuEhbbM2JAXMh3P0NNFjy4JEuTGbA%2FWgUAYmKDs76CjW%2BcQmJdFxvenBtZqi%2B3ToJ1xjaIGzUoSdglTWwOltRVi3F%2BIjgOdC0RiRhpESkwMzDgoaDJBjqkAQuMcQXQLtpylAP9uW2%2BDcRFJ9Sny0ftaVsFLD6zudarYWamMOtMPiKvH2pZfYKAhI8awIcwX%2F8gDw3qrYcmFBjDO92GzTcL1aWkJLCbekUAKJnAXY3oW58LPRxtWscB%2BgzvuvsYjlzrxB5%2BHiu%2B7ogQVNbVRbSEIpFlmn7TJMfcjqVchlbXtwJbZltgtf8wd088iE%2Fe%2B5ijvMlT%2BVcA%2FsY0I7Dl&X-Amz-Signature=fbc2a4b5c2f468fecb865947ab6353ab0e08c39ebb2953787872dfdba5714c6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

