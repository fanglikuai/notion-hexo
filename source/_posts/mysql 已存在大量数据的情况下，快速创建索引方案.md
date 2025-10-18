---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBAQH72I%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQCdmZfCbi7yuOxHM9smp8tT6pICJ7D4vLJt%2BfFNWqT9dgIhALEP6bSWhWSWaRaM4YyqjU3oYkFU%2BdXONnW5vZkvwwScKogECLj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyaYQo3OGWfg973HEcq3AMxX4P12PpefoATQsfqfzVni7xjC2c9B3MZmUD%2BCtqPvCCrESw5w%2Bk95wikbL2hID0qkyWhUyFQi2q8FNhb%2F7CvjaiDLnrFGJAeculHIqB68Nrf6FxY4HxWR3VQ00Q3U5rwPahJQBH2OcvUK2ZiPAvX0wOkFc1GhiHCmdYWqigWtG0l%2B11BAh4J71uMImsrKP04LSQaso9WBE6POAmkBuPmeCPdijn9DxV98WKg1bkdHPVP2ya72VMgeyaSwTc1oGrFCSaaLb5T3j%2FpZR%2BQ9gUVmmrKOji2VQyBlS%2Bf%2F0DNV6WgHFZZaCrr8A2SKpmfi5DySbrmfy%2FSd%2FjTU3pZbwJ%2BZyeKwguyToJmtzkVa06izTGZJQdGvB13xfRYUnkO1amIQYbAIE%2BLfNavwscQtxSY0eVDpdnLSzwFnK4sB93QV0AhD4m0OhrqRkakAj7jsH5qJNKAkw4qWoQQSQ6FpQtpfqysYoovzyjeL3Og75xHFBkmtAvL3jVzN4goJLXAweSk2g0JJw%2BUEnkKRWq%2BUiL9A6AM%2Fujq0j3xK9Tkm0MXbCK%2FTntQL7PxFo8YQvYC5NS7F3%2BLZEPG%2Fk0m7yD6cm9WvrWmQNBbuM%2BRu4CLLYPDxRUAOtap2bWRkDEuHDD85MzHBjqkAV1djxi769IjT%2FA583bVWU2bVxlpGx%2FS7h0q5G0ogNM6WwrIvnSplym6J7vz%2Bmh6DyBm%2BZAAL6zasXpUoPSHGyFGVWXfapAtqDG2XAe5vwul1getybbDcLjePIfcZ%2Fp9YHR6uheo5qrvf22V8FUpTXWJzNkJvVL6%2FsIXD9T5%2FIRUYbr8us24GKh9a6R3acajMAlAaAeGN2t%2BcX99CQK9sTgcFqa0&X-Amz-Signature=3267f6f200f39b932d819604729605505e334d621ef0ca27a85bed65d2a28b24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

