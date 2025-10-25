---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TH3FP6BP%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T220053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrAnINkiKuMGMbb8iaviM8oxxFsHJ3NJN62LKd%2BgGG8QIgEGpLfIKx%2FAYPadWCVUrADYzxou7HIM57HpKQHv5P1uYq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDKxpGmwSQXfKpUDk%2FCrcA8aKCF7idn1gn4FfnRoDZ4lzwKD8x91xCq43zJbO4n1lCaFnvNdr%2B1%2BqM%2B21NPCwZAV40iT1W0J0gw7QLGczbpvy09O1vDFGRMLNFAJKoC8wEOeMlG%2BPRLfxIf9kSBG%2BGpT%2BA86ACGd23zUw25F6j4OzGfl7XPN0FtQB6VWc5p5GzMRFdQaZ%2BeEqZIJIleLWN62wefR%2FP4B5%2F1qL6WlHc9%2B90nHkl6LxW2tLyiy%2FDpmsOIGO5I2XzEA08XOZSDkQErocQnSooAZa%2Bcz4Ay%2B%2Fn95UlxxO%2F00BrobEBDAOIpD%2FItWkNFc7S4X383gygIA7cljT3c6KsVqX7zp3%2FIp5l84VMb9EpeHJ9eZzW4xX4fjNMckN5iZH2onwkAZTv%2BIWGs3qJZv9RmzX0E4Gq4fQHNIt7DSL52ZzGwJrF3jZRe8zvCpKYGOlL78eCJ3R6lB0%2FcGBfZMs2x3lJcU2IVBqcnLkf0kX7relXPfC1aDFkjnH6cccyLObbg4aPcSfA3eousP%2B1Y2LLNG94ZgYYNkXgr%2BnCBZBeqeP26xqLBTM30ZF3fvPohWBf0rCrByZ33yAVRfk2ibwvRLiESVuFHx8BgCdQ5uP4TRywPwA9PUXOx4iR9S7IOIOKB%2BMgClzMMn788cGOqUBe41qSk12UgVw4ho3MBLwkVkYfULfaPwEaIX6inWGUNt8PhIENWUC%2FBiV3so7ADfFEdicRGUFR%2FaaEEIc2SduAYCPm0eqL0PMTqml7EVlXkvoZvUU9Qofi%2FbdHzrp1lndtWA0gG7tGps%2B5H%2FJk%2B%2BYdm%2BinxWrUOVzBfCiLu8%2Fw1px9zGk%2BGbHYIhX1HCcoDUVlKKo%2FSfQOXbPFhaoEbpVuIwyPn1H&X-Amz-Signature=c75dfbe42cfd23621feb9bac7655769a1586c12603977152c63ce46c7ef6f038&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

