---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STEDQ2QK%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T200052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJHMEUCIQDG%2BteVO%2BpmRqzG0p61YGgEcLKXSF%2FQPtq8GXAU6bykdwIgWMzHWc%2BRWBlnJsh8omOQBbOLRj0SuoCrVEHxr4C%2Bmqsq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDK1IABTxFE5FONMApyrcA4FcRRZ0kdeFD6kbYXH2EnNZo2J7MpYm7ZC%2FBv0KxqPP3L3fr04t8idtJUk7Cn98vaswXzZp1Dhsy1nAOM2iCG1VlejVMwE2d3otRg5%2Bf68GeeUvjoXBnHe6mw6i3w%2BfRc8gBCaxyrjI7TwKJHkLQ9TOTMXxYLSMnsfmAk3mbFeh19UCYNCY6uGHXAk%2FIB9IjJO936PkqAm2HV38Imw6hdTXFOYA2bfUBZ1%2FGLRC77z4xaVcxVls%2BIsZyCARfs4E0GeHuu1XycyggB8ot%2FmNHm9W9DwdziI4q%2BdEu5fvY2Dg53PUsjbfvy%2BvYXSGYeWWpW2R4fsLEk9oloS52r5Akt7Cy%2Bh4BPSeHCB98hvfEJCoqucBSiQAJV4uihoCVvz5tjNhKYanTGIRigq3cUsb6owbjzmVaeh%2FwAdJWaJyJtiMayEE0aty2mDfIcEE3HHY6qWiAQ4okppuExt%2F8VMOU8NRkBshy3efhhM%2FuFbSyWV%2BrAe523TZuhRu4kcZJNXfR%2BPj4wI5RkEtiF62RLq2oCaEX%2Br1RpiJlcNzh6yvLZmuBRmlgG%2Fn%2FqFq7a%2BlOtrYIz5uI%2BW0zgCAhfvZsl7mCqbYQgEJr1Na5uTJ9GbWMFyZJco2jgzYuCn%2F5jW0MI%2Bz38cGOqUB3TKfP4Uu2pFuJiI32i6UEtQEyouTleIO1H5SSYJG9HqlnxjaGGSmaXFKqx9RF%2FcouQLSD9%2BcKDYG2SsestpusfGKPouE%2FBeFtHYyq3Uw3Ht8T5ZQ22sY6v5alPOJ9T1LjEdoxnCIf%2FSLP7h7ASR35W2P3p5nRljqPpkFfhnkdbfdjtc1Ua1VzGwcooIC1CahWvHFjLicLE6dTrJkGmbUvdI49sY2&X-Amz-Signature=ab5c392ebf13672ac406695703148865ed96673dc6825b5a3b8e62d3a57bf09a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

