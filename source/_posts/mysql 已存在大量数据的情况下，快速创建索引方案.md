---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OHLHPLZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIANR9QDL4dBUGvlk4utpFxAI3hzAIVALJRZbc186S6N7AiEA3%2BwSBVniUqjKPd9N2bCtQL3WYpR25wX9rId47dunMLUq%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDChhenBz%2BIaHXeUzBSrcA4tASmYOCPjwm%2BiFvuMPqUC6rCF7HaqFp6f1lLbq4j1yCk7GkkX0VOBN6lL2GOG3NRkoB1%2FF6qj6ZkES0F464PeuvfAHLCaHGFIARCXk6QWVdVTUqSgwpPiNEenB%2B5TOqLgnfKv%2FJylhfoFaWFQQr9EeRmdlNWf8cH1XatMZDhCZrE8Ilm9%2FweMij%2FIzQ9%2Fo%2BjJsIA9K%2B5eUndKs4FN7C7E7Iy1xGP%2B5FOuB9BE3%2BiYeFPNQPW%2FYJTpU%2FLkeZF8eHXelwOQHD%2BTzVzCz9PvdhcXBCn0Ek6xkkBsLfM4Zvidf1emlNbqEjHoSwcNIt%2B33PiaqCa%2Fq%2Ba73N9SSdi%2Fw51idMYwDQGZpmgmV1XWoycbMGAlFhWEBXXogJQ%2BVO%2F3o52WxSMG1%2Fx1T%2FdbGKhLVj9hyIjA2hirFQDQf9NB2NJZnpfcraUOnTxrvj85bFchNV6%2FvsUesTK33PKqG2eAtZHtdh8p%2BLBPOr6rugHntTwYr94Ed890538QsTvmJ7jpIx0Bw9ADbGhxKlWrF2CtscZGYxSyXBr2r0GL5nhdgh1iFt%2FMkCoxiM%2F24HZTHVmMHoM0gCqPrVoEw9S9bfDp82MeG6ZqIslaaDK6s7WUz96kr5fRK2o4Q%2Bw3s80P5MPC93MgGOqUBM6HWhsCERQaYzgKSAeT7wznKsFlXQi4yhwpVYpgba7LeMGXN%2FBx7ZsAOO6jDagc0Zw%2FUI0PwImOC%2BIoi6c5JGHT9hDinIFuZCi%2BhroKofXJzbi1iyoAKPjYRqvMEYou60bbPdthF31iSPj6epnRsEkkHsB1lXfFEFG5bP5Ufrl2oYIy1VrCzT2UcbcPirH4fUfQY52xAYkTFBspcBGnzgnXyxZDf&X-Amz-Signature=6f28313b4e62a7ca49bcdba1a6e0c3a8fff5d2f95afaf3d901baf5ca2973013b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

