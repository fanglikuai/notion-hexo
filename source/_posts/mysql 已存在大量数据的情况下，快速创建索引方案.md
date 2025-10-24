---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RW2V5KLL%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF8HkDSDsg4MBjAjemPOmPMZa3lfxgCg%2FHKziEFEoEUgAiAVhwzaJ3v%2BpMs0sp3K8oUU54X0BCOOcvitN1rGBOxAqir%2FAwhmEAAaDDYzNzQyMzE4MzgwNSIMePCxEz5bGRqejgzMKtwDPuX9bpJMF%2Fp2ypDYYZJ4z5SX3muru8xmZMUDlOLTmloViOaRkHUFhUbPPFMP8AsgmssSetwdNZDdybZqScwv7COPtf2pyZ%2BVdOx3lugiEbGZMyLapf7usUZzEWwljzAPivON3cM6iS%2Bl9EpbH6sts84yNVFM6EyJtkRamT2GyYQ0tkuVrjK4hMCyj6gvyZ6RHV2Z33Bqv4REst4athBeLtBRR9DsdupzN4IaF%2Fm9QBI9xAP0ytr1WVi7%2BsNh3mf1fHCua1srd%2BgR5AgalJNWzvpnfaNNJTIvICreYlQF6a4Qw2Szx5xYyZIgNYl%2BrSrSbgruBMFX2NxUHvUawwedIoVES4xgX9ctvxOk0PammRqctrm9rmR0AQIMYYGloCAuVJU8EL7W6emFQHdYga5HYc93umDPXLAtlPgOEgNrDAieaj%2B9ZqKtCIrWg4Oc0pjoafDb6my6qOGJhnG1E%2BNEBWOfwfUkZySMAN95inEsbDRplob75hq0zHFEfscukcXnpydpf1dcfH%2Fx61M60dBabXp%2BZqjmWdi1YCfqez3ysGvT8pMHQRR0UyV4CEmj%2Fu4QUdv4z44P1xfAgDsQ4iuHj1Tti6VEBjfA6dys7OYyf4jvQBBYLt95rXcClCMw8tbvxwY6pgFQpWNQzZBr5cA2YVkjiZgcANZ6%2BMKBEgvTYw%2FtijbUbtrtGa4HtzEMP%2Ff2TTiwmelw41pqm3SdgE%2BJbTQ5K0V%2FYcOKkTim9%2Fququ%2Fnb15wVPupNgB6gf0%2BFtrA0SdzSKVRvYC4o28gO%2B3RU9nyjBxRz2oytH3AWK2AP5SjcoxodMuSPhR4EXmLY3ivm756sG4pDeo0J5QWkJaT6rx9H0Rx4pCEw7hT&X-Amz-Signature=4f37d794811f0976f063d1de60d990c998374b156103a201e16e1c31ec336728&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

