---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDGUC7G3%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFwnx54920F2HTKqBZrXx39WhaJ3fAPMzicevrW9x%2BpsAiEAp3drz0GUHXk18G5ExxC0f8Dq0XPeCtHAt%2BPiwttw8kAq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDKY14K6lJgIhUsoaByrcA9Sy6HspznmLtsTmq29wKkKnVS5mJcuZFd21SnTCEjtVKZN1xF6Z90aWPX0dmlT80UNjYGWsuDf4RoWyd7wfrsDc0giCNxS0FwOw3lRfYMG2MW5RWKLT0FSC2suM%2B3gebZzdkizq%2FNq%2FTDTo%2FuIzqpXwcMEt0gvUEhbzc3Vb6QoNG0MXQ%2FHYzFN9ZJjua2FTRkpWPCiGtao9UciD5VUv1oAdWzIo43OgC%2F2g4GOrB3fqSXYIA6QYpfMIN%2Fjgx0X5FBqueK2UnAJiHqpeeWKjt0FkWV6J7%2BzG0tdJ%2F3p%2BTKofuOv%2BSVPqBPTMAR1uHncuT6eAV%2B80ZaWoVhsF74FTEH%2FRXOjgEDmjO5dDExSHKU4RbjfhrR2D11nlzDDvC443UCmYEf0W%2F%2FP%2F3tYm%2B2DKPFxaCYNRnoF7iA4eInZE7AxNowmqPmqWamQgdKKkvIeEjLUrh3H6qTnrmra%2FFyCJDeRUbNj5za5uDbVnjmUu%2FvZjzkFIOuQ7nJgdADxWcSMk3ZpTNAz8F0KawlSAArK9fb8J05orFWwqivhev3xdHhXF0fLIazYeV2XTb4nzJE188heUBLeCxnMQVbgbVKpSoYQ03OyhCxAeY0%2FTqBbWU6FfIG%2FiUI4ZelYdcO2lMJOQ%2BcYGOqUBp9s2EY7kHZQfl%2FVNxVE3LRu8PpBhmzBujVzNTvKeMtrhOshd0Aa7qKPuARHsmDjbvZiBbpJB4yP5582HN3sFbNL8ZBO6aVBuMaxLVMzaz8Jo3MZb%2FEV2lYsz4Tpdk%2FMLoMOV91jmWI5j9pg6OdQbMSlNto77ZJByJY%2BGAn0ubmWmPBnlKUOAGSYN5IRvB8Y7eYvAyvqwxLCc48v94255JCoPT5Mb&X-Amz-Signature=a9f45ffa38c8097a5faf789adba7d3205a6c9059a08a5dc108c11d824688dffe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

