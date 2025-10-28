---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RISG4RN%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDnC6oKVRwGHXfZ1kyS3kF2%2FZGG%2BMPpAEC0UECo074TpgIgMbdMbubjeUGiLCSCnotxVj%2F8OhbOG2G7cu9OxxCy3EIqiAQIuf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPAoUgOvfcwYYvFoIyrcA%2BXUSYzEPPxuVRcMWhxXX%2BPWZ4sqB2SYv7Bjh%2FwGItJjq%2FU7PjhFdLDEg8my1DCIK62MpurHG36Il2tlvLLdHH2Gyvk47UitfzoQPwRl8cHjdtmXcaa13Z7OtaVWNcfWiwaODCvNWv%2B36Li7nw2EXiUMp2D6%2FA0vcdJ8v%2BGiASLIMDGvgWk1UmF6nE1OueTP5zDVfLoN%2FkupDmVNBxe0%2B7ufWqwZd8dSFuk2Q9gGqX5mXBD%2B56CkJFO3jOdPp5o2wJaniofKLzsKDqn%2FbC1j5mqMkIO0skRC1rBV%2FWwBipi1DEYv9szPUea%2BOC%2Ftq7BFKzD6AyTLP%2FYKLfwmiHmDjtWOeipsW18dImRxrBSO%2FXJV0IehUw%2BIxNk7mHcHNZIGkKZOo3IdJ1Bj4qdJwg49JIu%2FIlJp5sS9O7D4QrSmiNqckf3f7CrlSIbqp8yJ2mXcyjZhAzChcWi1JLm3qVdNDg%2BD4u01hdEhRgkTyqstV6wHh6icZHhROhy803T0UbPnb77uXFU%2B79sYwQP0G9LsCXvI1S0fR%2Bpfyd%2BPShJvPDIkC3TeoUp1NgpYntUZqgXlbDBNjJTIVA%2FvY%2B8n50k0nrZJuMUsaxNz4NbhEYAugK%2FxZuIligoJx2Ixsix%2FMLbjgcgGOqUBdevUeGpkCfyN1tB%2BqUSgLdMRSaHovdXcHHgwYvhcZOyn%2BUdN24Pg8d7aJ8CvRLAvK6TZGwRYwZt28tmjRCJmBuVtYWOazMpt2UH6fRUel6tuPC%2BxVBfhwi8OTXHXgK9GRczFMLUGsUaBAanksTjyVxAvms85o%2BkNCjjK84Px1R8HpYyGfNHx7IHNoK%2FPXOEi0ZHI7KKuvRhad2%2BVDj%2FH7XcAtYmN&X-Amz-Signature=722834572092b7905665ffb80860d05462d31e1de60d77c4a7abb4eb1e2bae03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

