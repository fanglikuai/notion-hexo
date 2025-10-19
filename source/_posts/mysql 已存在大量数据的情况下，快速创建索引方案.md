---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z5S4JIEG%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJGMEQCIDWJ5JOZQjSvJhp%2BIIE%2FRygywqImWvUlrXmh1YEIbDQTAiBJULsr844QsalM2jhm2lF4%2F9xKfBINcVq%2F%2FEcu3uGSbiqIBAjV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxytuFaddy%2F4K9SwzKtwDKT%2BSB42PDZgTbbysfq1BS4zFCh1ECoOgcyo%2BAAMPuM4qdx1e%2Fr5jY6BH%2BVj5BbtsETnbe6%2BrEqemnkOLFdxwGqBh7c8HXlYcQQJZnuvlkSC%2BtGS1VhZuCSVBx4k5cw0z5FDgGHihIOXNHxK1E2x0cuu82XviCysqwKg38gY%2F2a03lvWfEu3mE2oRd33rPvv431oC1XTpzxUP837%2ByVI9OqYzqc6UZeZaG5dmeTC3UNcRwjxkJD38R5Cq6yy9H%2FM6q04vOGub64twewBcQby%2Fd6T4LS26uEmOJKwVUp07xsSPMY5tlQM4%2F%2BaWqgYZti9qsrdZwCctB9%2F1%2FuiG2irRmysbE%2BRoAnREFZf6Dvvr3ogBdAqrbed1D%2FdGclF4PwNNNnjxsGRvVg6N61ldfOZtFVL%2FUb9WHpMhjitQVBsnNXXYSTDlDk6SuPZ3HA%2BDu1zQAjCIjHw2%2FeWRZegH7X%2BYOrsi3ZBaQ%2BpjpKI3QkqSW%2Bsv0DG58yGz4wo5HCA3jf9vlFWc5FVUbtasXVrrGzCk107KyqTyizY9pYThyFPYAt99SeUZQD8dYQt%2B7cudjXsGQdHInzvWuXs9rh2n6c2fw2ULL6H2VNzsdp%2FVefXXy%2Bv8hfVMcVRehUPfVNEwxabTxwY6pgFJZ4qSyFfSqRA8pN98OVXEMDY57xD0%2FL4klh35NCsuS6VtB9RlmRRAsVAvl5A6EDglkiNF0stjvGOcDJGjn%2FrxAiYT7OM4wUli5pIx8uC5Ing5hVjpwA8XeIh43OFMW2sjXz1EPJITgIjKOouUUGL27L22jfyDDfX4bRx%2FWgqJQajJI55wjXfGodIZCljD3q0SF3pblyFsGB1bI2Yg8wbDH9YMhyYH&X-Amz-Signature=8ee2c1567b47a72c7500f331fe52926fe4ec674c935107e3cf6d944877cf7eb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

