---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466446KG3C2%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJHMEUCIQCmMOzcRmxlJNkSjv9OgB2lTq82iochha0M7K5ZliNCeQIgWqMwkbXgj5dm7kicKb46ZymEI2CRaL5LkvMWj5x1rFwq%2FwMIEhAAGgw2Mzc0MjMxODM4MDUiDOI4h23pg9%2B1YzITHircAz6vxD1fnBFr25N7Z6hbWBp7%2Fa8sxaTv8ZWw1KKiU3HrllhaaBsd5adlbXaw9vzKXyl%2B9MoxktHzVgLqNqxYm2DTalyBqTnwlpb%2BY1p79SEdnSJYDWcPapSmo4VSzrpGxirISP6xwGggzKCnuV74inLkaWihkhgCGoL1Af5b5KQN16%2BUUAntVt5Dg9WVTRiduLMUB%2FWVxv6eqrvqB29EfsEbajXyzCHyUYO%2BHyCQtGXJ1c1QnzIl%2BM5M35%2FAyISQ3YiXogwX6RtxeaNW1bgg7JycHqcu1vqxkUXfPUQJ7m9hNYHkrG2pTFsFJ002EZKdWsS0vJ1AITDEA9gBrufsMmLh1Rm6v02qoBFdlvFG5%2Fxu7z4C%2BdHt7FowQq%2F0TDwa8KnAFDQwy6sFPhay4VvqfITgbPU64nFBkQ%2F8RFsjnLccXiAjyqonuUYm%2FlqMow6bMM9pzluxv24xj6JF0zs%2FEZnz4sHEZoouHsNIeUML7%2Bdi67BPXUEZQ7mZ%2BqZ3ntwa%2BY9FDObVEbV%2BPfzIyu507O3Kk8nhErkh9rdb1QErpijU3Z%2FHscm74x%2BdI7%2BV%2F5Y%2FjaXCP1ycP%2BMiyG9ytKD886b2Im2MroFfGW0p%2B%2FcIv4tSBavAHfbL0thWK%2BFnMIKU3ccGOqUBgbge%2Bb1i94IX8u4lcq1bqTkKndYg65vAmCfQGNvbsgVjrIt5BeZTtEOML3Oq4OWZHbV6b78NsSygoCzFwZYYv%2FxTdgWWPWkqA9dS9h66t3mBYFEZwLzFuEy5SFMvCBR4xh%2FliVCAZU%2FJFX5JlADtAvpOce54cMXYESRXzFQJ987b8AEXFyNkACnlnp1CKNfTq0iKUOr%2BWOfJosICyIIpgX62YNGg&X-Amz-Signature=77ab5c100841cafad148663b38098014674e109076c0cf0106a2587e753d4bc6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

