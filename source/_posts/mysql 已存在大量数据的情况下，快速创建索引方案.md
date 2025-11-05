---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCIPOSRH%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCyEv6aAVkkZ1WH3WgJS%2BJuSj9JMATFoOsOSrevmdjjagIgekUQhpvuc3ty7oAhTaIYkXIadvvhiS6QiTMCGIHS%2FTIqiAQIjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKgrkua09hHgwnWlgSrcA586USsXUfEvmvRKfUoma1E6pT0GT3I1eaB%2Fetbp7MHkmpkGCEKGyQq6FJRRlzE0Cq0Z0XRBaqsfZtN5gyjxVlxjWRslB5mNASAURQhQKeGbBhidbMz1hpE34H9zckNZ5CbgnCfuXwVpnl0SIwIuv4E%2FUs2kmYDQW%2BOykztTaUBxbdJ%2BoNXu2ZtngqjXNr3zPWKsEqo98hK8Ky7k7Ih%2FvbhWLj44%2BcpROCUd%2Fs08vwRBFnioHT5tTKYHWz%2BIhxoBDVxpHjRWlh3e%2BJ1vAc0tWUvZGvrD5Ie%2F96CDL6p%2B%2FthTNFDAV5FHbfsiJT%2B2DXPcP366V20DfzTWZHra3t2xQjmS773iqrFgUeQNi1pdQiAcQ2P1Clc%2BthMk%2BQ2V2Jvn3TBVTHla2D7ws3%2FR0Jjl5UVIBWrM92QXsodJbBHKmrCCrxLJULwWDdV5jQL%2BAhWhULIswJLrLZClkCO27U4Z5zL5cfgZmCtwkEdT6fClQlz1mZb1pmpeJw0IK0dt0TAmJ4EFE4fBPgOE3xOUlRFN3kjBc2r3OT6601Qf%2FPpID5W5K0ctxiYZ45AAqgQtnJwFx0s%2Fg7G18Hv8V%2FtsntBkcQUS6p4VbyxJjAsZMJckWkcIZ0IVboFV6FXnPmdDMOmTrcgGOqUB0tDWKdBANPNYUrMuzM%2BZ6JXmkGE965%2FpAKWh%2Fn%2Btir%2B1C%2F%2Bl9H%2FHMtcnfW5SNgl6SKsUdtokXng3XyEWqGBCV1%2F%2Flz2DjfzsNBTIQuaaq%2FAG4%2B%2BEmtzl8rZJZBF5PdPYGr8ZdekXd9g2UdkkO9DK3dAY74zwKXdRDFOQR69G0Mnp%2F2KHx%2FZyJ3cwxALM81mOw02GDBaNUFmssXzsMfRYyTGi%2BBee&X-Amz-Signature=40b8c6e4b5731ccc31d29a8e26b25144ecbac3c123d151e2672eb1e4ab29a240&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

