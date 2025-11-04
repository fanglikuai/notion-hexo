---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYA4SIFJ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHSL1s4Qb4rfZYdo%2By8OGD4RLHTbgplWeUfgPHbfSRn3AiEA4iMhESZtATnerxn9g%2BxmX32PLrZpL2iDZ14ggRsnYNMq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDMTxTAycHYuuGIMqfSrcA%2BGI3axjBtLABn8OW6YaOmiG3GrClg%2BTpnsJroYrD860qWjJCIF0iqrZN4OobofO%2B5hewoONelLYwh%2BI9ZWaL8dVay%2BDdEhSp7G%2BR5UGap5%2FwLYvfcBq%2FAgMZC%2B7H1mXdm7NddzEwI16sKqHGlUihfF1606Z8raKUdOsAQUFN28lJG2lQuYH9vGVpa8rpCj2VraaA86TIK33rkYwkTUXgOjebqQR3O233lkvMcIp1c1cIIQcvJguSqrSGWbSwqZmsGzDb80aOTlHsuQjbANYEDbcy8j9DaccnNgvEStKfRglnH3sGZfGHTczL63hLjWkqkYNWkfYwJZwnSyiGNpJd9it5qkGdFoWaS7tMDZicwverbluOFZfmhNP6xaVVm0AZevAt1EVNHMU%2F4BocD%2B3qc4h2AaAuHheCwnF1fqkZwJpuJetxPYyAsUXhrzFSlOrXd%2FgoGA2ipnAq7BuyK0ybIYcFWSfVfJ2ChvrZQabbiax1kT5z9A6Nd0AVGNdPyX6gWPFlSyE6eAYTCDIDcXV4zPan8vlLAAzMoKVi4l%2B%2FosnJNjXjj9UTs940uqWwMlTTAtEEMN6wR9kUhoPrm6QNHNhyASe9%2FhupyW56dXRx72zUGNt15tV7x8pu0DqMOiqpsgGOqUB8Jvf8N3kwYrbuVeHMsvVwLoescPBQDplK%2BCsGrdXVUcFXoHGToAE%2BgjeyEw%2BU4KUX9sKDOezW%2BDJSW9HyKNcM05rn0%2B2LQ3vQkRa8KeRJ1wOl1vYMizDdEd9vsB9QX6Hht6iqiFhxhajmeJpgQI9ocxH41rOSY6s8E6jREStYEedt0hShjHUtEm5Mz3aOkcFC1j6SFetaTsNO1QAlmFYiWs%2FYOQL&X-Amz-Signature=706619dec1bfe60c5b03a63da6e204a0f5d86d6a02c4b87d8df766ba7b659d56&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

