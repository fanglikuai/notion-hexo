---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTFGAQBQ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCID2xhC04ggTQn3XDMGCS0LYVj0o0vn2ERFZ5wEW3EeyUAiEAhE2pnG1518iJNJfbgYufta2gOvTJKVnXkxJi2P23lAwq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDHpK2239eMCCLUZAgSrcAwJLMkWCbdR9Fnabbh9Y8Bq5XCDQWpmY1BRzYRF%2BGmH%2FQRIsmpxEFR7tFoVmo0R9nwLBBpjxHNR1hEyX%2FLq5iOGuq%2BH3HN52%2BQpielqwkkFjFKGpQfNFG%2FfzuE4vli2wk%2F3IG0wPFMKjg%2B8DmNy91ZoKkd%2FZVkE%2BxByn8IgU%2Bv8KNAL78%2F6%2FImNRjVE4QMn3hJbfhVQK3pUYHV9AFCcgjLYzkP95lXuh2VdbyDjwSdMwwNmRRR4gBe87voRPdJsYp7X5%2Fv9vhEzzF1ym9c3O1kqf7FhYVNQS5oXhae%2BtHB2SbSKTSRgBPc7uAkCmxosNcB9TVN3E2sSyuTahDrrlnyHczhHbokmu43xK07e2%2F6eDZuQP3p%2FjUtf0Ea096jxWd0%2FQAuRqTuQkcd4lUjc%2BdqYI4g5RPyqcySa6HRoW9o4sbXXicdpGZcwfHLgN6IvXviS4zPDbWOpHJbEkpCPpJI9GrBXIix1RTyLczgw74aAf%2F5ohauvBTA6rwqxr2TGJk01DjMJxl6g46EhAzbhdyEeBRtyxMMH7ufv%2BNI%2Fuf1Lpr0SKyYrpwcTyu9zR2RYPgxkPrTa7snGXglnJad83pLJLXeMSy21sTwbK6b08WLzz9WSBzG%2BYGRZNm%2Bk3MOSy88YGOqUBrfrrG%2FA4rR1Nf2MP1TLjRpY2DHV1LOXdc7gVEPWjFcJku1px08M6fnS3zNAHVG3RBZc0e2iELV9zvxjbyRZUojrLTKglsscgwcMJ3Hqo%2BJR1Cjd7bTOrcRf%2FhDNEWDR7KMCsujnbpYa5ea8KL1YmR4PLA1TCtp473WMlRrcWC6QVDcbRYNtwtSbc9faF4nIWt2hU9RevFi03PD2fS9pyFhCh0E7J&X-Amz-Signature=2bc24eaab63e6bc122523ef5e64f0720109ac406c47ba1b209dbcb230b21f164&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

