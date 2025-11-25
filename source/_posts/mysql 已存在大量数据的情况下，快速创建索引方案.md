---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ZPEOUIV%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCI0BSyGKOSo5tKQVGaE6yMSRtLDc2u4M6uoscsaFJfxwIgBKbISRGICGbY%2FctFDAoE2mbikbIM5xzahR%2BOvXjGGVsq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDEjJQlduw%2Fiz1BuyoSrcA3MSzaNUNCWh5Qsert%2BZI%2FlTVQm8Nc6NvBnGVHZnwJZj30vYiX2yciwkW%2FNBAnCk9M%2BOj2V1wTxb%2BMfp3rFWjEKnoESMAEkX5OFi9%2BbefEdQMOArt1kEAU0uuOvwcDXk81L0hZlL%2BOnsr2LNHKTUPFm8kM1NMyYbKMsNIcui2WGEGa0psTUnd0DhD%2Fk2kmMbedleo%2BT2AXTzgwMcNkm64W5FDdrpQ9RPULYTJw5ZrLiXRhD5OwwegSj9MhcK0no%2FERk0cTSwehNlyGBNAbPqfNKxMXgY2s%2FaR6ibAq01kJKBoB26XyYLD1xYfhUrEowK6yN7wfPbj7mVuO9ugYVB1ebW4C2GG8EW37bqv8Ss%2BsqK1div41SA7D2a2Cpo66N4FnH6sdmxpVOKlIRoH%2B0BQbA1H%2BdcHE2HbPrnVib3Sz%2BcJ5DKOGr%2FjvsocpccV2EB3q46hPaxCNICaNJD1Dw9qt4Tz2QEFy13QM9%2BMy8ENrwtDqkvtxCpjQ04RkS5frk%2Fzzi6B3waDF7cvLdNdZaQfDqRxjtDlzHipJFx%2B9kuhVlQkZ3tRZDCE4WX%2F9QwJA3zrHob%2F8%2Bcd2q1pBZrkN7zlBysqoTbogCl7B6But%2Fb30TUI1s%2BHHZ%2FdBU0Ebm6MJaalMkGOqUBaVdl%2BrLW7DV1gJM0JohCUyOMF5qCsW0j3t%2FJ1ENr0z0znIZwI97IlzeDCtAni8%2Fi%2BxX4icxKPgzrcPgAhP5JczBNJwXEQOUPznDOW7v6yaSNClqUg8ZRqUCbFOz%2BLsFBKCTUIWlq5VHgVnFA4XddZqnw7Q8DB02fNy%2FvNPcqP%2F8wsj6q9emelhBsTM3YwT5KyJTizsaIaOn5DOE%2F%2BCHnD9Zh8mKF&X-Amz-Signature=395ca35319e977dfb1b7462ac8244598d07cc4c539297a0ffa742191b8bf1716&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

