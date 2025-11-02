---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTCJ7YN4%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDFXc7STralWc4tOyf9i1i1GPcXi%2F1Sb1pKY9JgNLhQigIhAKfyksRoK7Y1Lefa%2FFP0i%2FJ5jLFunpAWqcQo7vMg7EqrKv8DCEQQABoMNjM3NDIzMTgzODA1Igy1tE54%2FcFRVzNp%2F74q3ANE7uiCOpRvnEFaBh18kFR5WTFVaTLNFsngXlxyVwynnJpOfEzqY92HTuUoJHEA%2BRd0lD9uCIqrjWj9cfFvjl4AtBjD6OaPPTLwKeYUBCnwptM5eo4c0WwNaflFaMgRpBir7YANezSHyL3%2BCujzKmgQya964%2B3Ly5aMPchuZXD8PfCdDVmaSW8%2BOAeHzTvDkFGxwxkCCUxXWpVDnorySW1avB8oqRsVQvHOPNlYlfkEI%2BYB9l7CNZ3CIKTB9rVGguwvQ2aVVrQoNaSA4xXqiMote47al1vwOeQ2ol3%2FMu6oDrRrSxAf92jwdmkls9i1OZE1M9%2BDXRtU0coZ%2F8mg8FEfqMC94%2FJiqEtvtZPIDBlmtMyX4oIg0pX7ZfhpCH1lBNjWtqYgYGeYZbhsUwMetKoJrLewkAF%2BoLr8jVDJKgr2Ml4CHqrRNLuBeuUztieeKcO2D6KmsvwauWvgptDooMzxUYP9C3hTAelvCldFsFNlt0a1tFwSeGIJpwGCUy7HwWhaA23EOrXSwt0GOxYOFKZIgUAV7zzw%2Bu4FSMBKVrx%2BzaA03Qq2BSDR1BUj%2B%2Bf9W%2FntAIa98D7Z9zZtQTAe1mMh%2FxJ7Juu0CpKDU16GkZTXqAkL00YUySt%2Bbw3EvzCI%2BZzIBjqkAefd5Gjowem1Lf%2Bcxl%2FS%2Bx%2BtpXrHlo5kR8ITvbb0ahzKWiO1%2Foep2FYE4vfbMxx%2BWm%2FbxBIrO1TQJiezeQHWy5S81HZ2NKUPXJjVmGReRUofYZhhjIlKe%2BHIsiFDihqK3MLYfI3o8ZWLPYfXIfRRnGeFU6%2Bttlyx2e6ArgwOoZENj2UFlR1UUuHVewhN7rXntzjQ9dhHxdooEZki%2FFZDF5VRnqLx&X-Amz-Signature=e10a6a1954eeffa4f4dc15bf115f01e08e1f4979bebd47c951fa8ac91e79feb1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

