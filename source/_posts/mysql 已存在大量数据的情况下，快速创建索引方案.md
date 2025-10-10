---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WIGTJBY%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQCJH8xTjt%2FR0DqKW1iBOH6f8p4VrGDXiSBor0tr2bUOpQIhAObNCB3fZkPA4PcAAwVJdpABGFIBPTVUNvwZ%2FQDN0qyRKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzVcpxkMXJeWnuImT8q3APTtZF3wfNbfyJsezc9Z5JYORXrmA2RL%2BJxEQtjj20DfRa%2Bu%2BvSxV29WCFVt%2F8uaG1PP5bTk8d8Yt8wTUwwaK6Vdc8IU8T5wJYG7NimUdnA7a%2F75b1MSjfF5syneh9zsNkO4OQDy0XXGbAxeH%2FfTF29rxEMxrgMhPx8nhsjC76lpGFXzts6ZUcP2x%2FkpMpcbqlv%2FcbuZ%2BdW%2BwuxdexIMNTG39S0ugTg3bdjB2HQM8KUDUqoDgZgHJhamTXtnMvdh2nd%2BFNr7pE2MKDmCi6L1jIraTEP00DW7oHPP7wi%2F14Ul88y3Xpq7HCTj1cb7nGD%2FLWcNGjDoypngkwFY1JDaMvuexEDKpELkhZhOP9%2B0do2a7XtQXyl7oI%2FVTajNXqcr5pr4d%2BWn%2BE%2FKsy5L%2Bl5o%2BWtY7AaYGCzWKw%2FyIU%2B9GA6JCxs9jb7gf%2BbCJVj1CLJ2o4owFYVTgQVKKGQkphS3vxC877MuYaU%2FROd3sK8r8zI9egQMK%2F85z3YVanqOu4ZVfVrG0bAvDvrr%2FiRoYj676hSg2id5CxWwV1I8OOORxkG0IQv%2FY11qT8%2BIuE%2Br5hOjT0IFROU0tW3riagGIf%2BDA0%2FIdKwqkXpRxFDl0TJM8vQrYrIFLiql0trJ%2F2LDTDf36XHBjqkAWhEGmuTkX2jYsKqGyrFC3dW135uC5LfJ9%2FcGBU2Lm8QXykk5tPCrOC2BRnL5umT2hN%2BUt7GCGM4lD5q9kLY1A9jAVVICkRMpHyYh%2FLaaS1JJBrSx5T%2BQAA8rWZL8%2FRFkYRrLdTvHz1Knljn%2FS07Uz1yPhtQ66FjSJQBocSswILsubuiifkpFMlFxlycAfpf40iWshBh%2By7e2NeEaR19%2BMfoNVDe&X-Amz-Signature=47920701d5697cce06a102737bcbda8cd3afb14c95d2c2c18e5adc9fd60343a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

