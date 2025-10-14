---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AMP72RT%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCShs5XHSPE6PP8XqbF4HAX5C7HpmcsPN1axFxIDFFYzQIgDPSnLoIe7pkpokzU5EkZsuAOJUnc%2BrnIElqUu1wYNFgq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDCWhRRb8T%2BSCjSUosCrcA0KXOvSGUMga1G3lbArB1YeSW9luJ4PX1uzS8HUfVcX3fLEkqk%2BkYIeQ2CyT8AxetIYVF6ovPBOQ6aH1NIjgKrsGOeY4TAdLL5gPjD%2FAm4gG9Zx%2Bw2PJDiVHcCXbQ1xwo5zRjm4wTkTOc2ATo0O%2FwECJ4HK4p%2FLAyGLUE52bXBuoiLeT4eIxV8Ab8F6dUCkHjPo%2BdepsfGtePKIXeU9A%2BEoID%2BfRXUujh%2Fujh1E%2BYJVccwdEG%2BClcLZEFDkpqm6wuxoz0NASVB3yg3wZFbfuTUuQfFE%2B71SKSYB%2B4Ng2E7bobNkV4HRXeXZiDyF5jr%2FAtEeTjb%2BOMyO4%2FYWsTvfGnJ5xYs4WIkWRpYs%2F99a%2FdVdimwBROwdvkm7JawbCdaNdKm2nVBUzDP7Fdbe4QosMoVW4IJbFH9EpjeWYq6THH6EsbarkCFhmOVQvuzkQUqXPXLhkO66nPYBsHbg2xqtxVcAZM98rwQrrt%2FE%2FQuKFWnXB%2BVMjee3s41FbUf%2BwQqHsxNThwrsP39Pfj7jyCOwpGHgFrdMRLFdUDm2eEeUHAKpkOTeQI92zPz727JpsRisay71Ja9%2FNBKgJH2jWyqUcowv7h9I%2FFqg1HzW%2FLEHa%2BTasrWSARBSRF0EhTs94MKm2tscGOqUBlD4QF1wlwRrDgnfKKT1wEC17%2B1v%2BtXJ5omo28nUAnbyMpEMFH3WiyNAjxlDytA34WoIKsqynXV6Koj3IRbvzY6HJwtRs2VCf9NDEs8onE5A%2B%2FoyM4qEVQZeTG5yqJef2SZ8%2FQoNPv0jTTt0YbZY1g37NqG5kk7ko1O7%2FadKLkp3%2B5OsDvsFvkjKZ%2BDZrczl3GfegAhnOJ6qb0kumDvH09X0fK9XR&X-Amz-Signature=aded288ace1a24e33b8c7daa64a14c3f59331d4b63594bbb44037de647bcb457&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

