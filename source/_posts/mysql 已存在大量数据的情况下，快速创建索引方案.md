---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PRS5DBC%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJHMEUCIDivo571T7r9OSWxec%2F3gy9NImOIutpojHPXUmlhMnefAiEA0QJtHoFRFg6skauUm9K%2Bv3H9Gk6hABUOTdQjQxjKbjgqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF38Neu9FYVbPcSo8ircA9ijYB2LHKlMBMyBWfU4UCmBEpplS54cMFtNq77VTRyrKZNPiIiplz0ffT8xHhKshLqaJVP%2Fzlx5fOa%2FyihQ01WugOQ05CrVxj5b7hgnvXK5DjT6pni1%2Fg9G3kwq%2FstbwIVE52vyevr6DfZgsTZ4YgFfih7aLjnWoWtnS8%2BMvSp2Z6ohXdhkdP81y528Br9paExch6Fe47wk2VqcgqxyWCRZYeo3%2F8%2BQNZfdF5l6WcU6hZM8LtzcF5LH6haXTf8ZLDvPBdIfR74PCBNyvgZx0J5ZNLT3Stm9FVuD8eTrNHWZ3dT1mu7om3eTjC22JmY4mEl2HznGjk9afLN%2BerwbwRjNBKPj%2FF7Gg1kIcKVcrGIEKzDv0%2FmoryoE0ww57pkPIoDxNFB193DlnIz9fZNdkOYkRJSNqu836TBabGhxez0GH%2FIeAvbyyURMryhRCV4LVy2IYZPyDpZZXXLMhX9MNy%2B8e6Cb9%2FDD3fQE7F4uc%2FbAUoxlIXcIBg1nPPGhRx0u18BIvrefQ5Ck3YdO%2Fvl77raq15XRtrghRMtpxA04ohaFhr4VA73Nq%2ByNrAUbn9fNuYpjogSfMPI%2F6otc3qXaR4fHJiQ10FLnz9mgTCQm2eBXJ%2Fcuzh2kSWYmS%2FDYMPPo0McGOqUB30OE%2FMa5g8X%2Bycm4aUDhb5%2BfxxDpJn4PM841OH1JndaotoXvaMcTixAPMwx5pX0SwXoOSMTtFx688LdFFKsbCV6stEv9Exu0jpudiiDFrdVm0nxhdDlEoF5nCXJyQv8IUR3Vq3HUBw%2Bs2yD2J9i2CKZcNC5UJP9B4vNGjCQG9InwxmOgUuEUCplBYYFR%2BxLucE2hVtfdK7ZZe2TMOhz2PMbTjtoC&X-Amz-Signature=d19f0a214425baff6526a286171e57100d0eb2c66f615b20521c3ad7ab8f9c6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

