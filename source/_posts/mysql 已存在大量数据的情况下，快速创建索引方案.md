---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46623GEFVMM%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T090052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIAnTzovZI4oxQ0rmDFN8REjClj%2BGnUf3dUJE0E2CeEpoAiBzurCAFHDrKL62SwaPclPqroP%2Fv1Q%2Fgd1FFSer%2F0ByHiqIBAi6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMRPzoKjO%2FFqYeEda7KtwDmaLdt%2Fhx9nPhakb6H9DRWf1ve1%2FsCD%2F0A6MdKh0Amp8N4d07hiExa9M0G25Dikh3DRFcNQVQU4SIfuIiGCBOPPygzA8JEcYquIUVeshpMWIehNYm%2F3BYGxcDxoncAt7i%2BLsejq%2FHu%2BA5lDRj7EhHDstd0u%2FmIrx99M1OzUYCInNAiikIU6FK2gXiS%2BwBbRX7N%2FIYrAkVIsQLR4Ozlarwakv05aKtniS696LPxl78ct8P6j0fi1L%2B4%2F%2BhE9j5hOi0HeAl%2BjxlKo9gcj8yRyD9DUB0aGL8Notm3cwf5QdeY%2FQnFff9oKwhRxGyAFI0sFM%2BvOzKud7Uyy06Y4XJQNvxna8HYG04gfO7xmFBvMsoInvsTiydpZPPrbY14WhA5DpX%2F7dKQA3EDm5%2FJpEhqnh2s2eUg5YuOoki%2BbZOklhR2qsUJ3zwzUZoOoAy6qyXIbzp%2BXD%2F4eDYtY2ZfVVQSt5A6V7j0jVJdMiGDPUCMjKNrro6bWA9RZktsyySpCQXlTitxZLOo%2BOieXPQXGyBUa3PUvdpJay32%2Bng7YGm6ro2yPyfftdIc65eSK329jsEA2LQKahIiWUPVp2cCm3SeraaDg4ZLwF74Y5iDbq22t3%2FmUDnRNrD4t4tBWBe3V4w%2B8GYxwY6pgFHIISz1mazwkjTNZcgQfVjRIbIiKlx1dP4sdKi4csKdx7Nnwn%2FSGVFeIaK7gOn9NfHWhMb%2FJ9PvzbwC5uuL00js9OsPFaZZ%2Flr0qsm0nh6D%2FcYiaYh%2F6wDSpXRulausrVkjZ%2F6z2IkoMIaboxKK3U3cGxtiR%2FK%2Bv06Qnhm5BU8v9eH5pHbAv404ij3ihwZdd%2B1jp6zdzGO8JZTBfj8bwkAc2PocY1M&X-Amz-Signature=069fed985a1f7326d8202662a886aefe9c5a957c5879a1a20e7c7e733b8dd063&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

