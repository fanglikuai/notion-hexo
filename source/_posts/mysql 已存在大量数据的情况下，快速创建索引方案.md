---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2R4LNKY%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T170053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIAivNQUosGAaMF4jpd7ZzC21gPho3sUqObZq%2FCbY2ft9AiEA%2BDyHW%2FkXEO68CUWkoI%2B06ruzDqiGZzJ1CbwjbSe4PdcqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPvJ%2FQ6s5C%2FMm0JMzircA2VyGzzY4i7sfmtXGfqxE1%2BO7A9BCfa3VJNd8fN9iD4XzzNXjN2VfjSmB42%2Br9IbwJ5weNMREUotmM%2FA4mXH4YoY0MOZjSULpid6ZVgI%2FogrJjNADqm%2Fzk5rV3%2BkKvqR7o%2BOvhEPFt2pEnGwuvPdTnwbhgOLXm7esJJya%2FyC%2FstjkVTE9184191hQYe01zuajtjy5CrFtrAHy%2B92q%2BNZIOJClUU%2FEhSVuT8Buk6DKfNg8TKeNmflR5kdrsAjlF7nbs7%2Fa9gbKoFSO%2B9R%2BD0g0MfbTeBH3QkopyLjsWh0oxLJrTjGScAbWsngiu2%2BeVy9iRpZojg1BJnjxgHEeK9EWUqJa7iwgUZEKEJG%2FrR8jgD26t7Cywat0A%2BJqhfdes4xWEG1UrLSqv%2BA6m4Eq3t1lRywzj6G6P3cmSEK85DuRiuhMz72inFcFNe%2FTnFHTjUwoqUHmJWibFVHEj8jxZ%2FgYLOte15d2lSApvjQCN2Ds1ZGPYF9TPVndwiuqozDTF00ThxXBBI7j8rk2lf8GVT8kE%2B8mx2Lr9sarMnkwsfHYPV8FoYKGjxqPlfUTrKy9nH7FMwrOxBDPTBaH%2F7myTj%2BJLL7WII7VSIQu%2Bso7didUV9stcMp7zIsb3Fjxj9yMLj2iMgGOqUB2a7uwjmnyn%2Fg%2FZIl0%2F4TpXp5vmwVTXNAiLjVxrJCiMlCn7OwdID8NITx7cBAkBCM3NnhqoPi%2BjC%2BPPvgE%2BBPBMxvXH%2Fk5x%2FRvNoInoOUVa%2Ftb%2BiW%2F6tl4zIf4dsfXbiq%2Ftz7wMC%2F6kvhQ3yi75ATQ9hYq4WRpd7gZgroSEvoWOQ2%2FRCECms3O0EyDUJK4m2EhMbeljoqsstf%2B1dQg959SId8TWUe&X-Amz-Signature=d66ecd39a3978f8134ad6836b363ab3f9ae1d376c06c4e6698727e431ddf6af2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

