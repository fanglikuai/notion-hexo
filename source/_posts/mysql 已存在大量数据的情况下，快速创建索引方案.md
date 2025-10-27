---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPQDMUME%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T230036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGwy2o5f8%2BBEqDHRM3tUbygtbFVvr5c1Pr5LHa4ixP1WAiBW6gUpo8gNOHCFTCA0Tszn%2F3vspHzlQ3lH3CaOihbtCiqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM89kTXxADQqY4Kl8iKtwDK6VMFW6sOTaQSAccHWA82abR4RM1DXB8esoYfWTFgUQOHViM0NyylyDT1Gvi86jv1UYCn0iFcsin3505nopxgDuBAh3AFq8lS9mHVmOMNaPKulQt3guwu2U8yauA9b%2BuEQ46edSfLk08KLltxb%2BuVTFxVWTuGamShN%2FcmAori%2Bq5J1V1se5arPu%2Fj9yecw51lnvDDoCq6tjCGn0eKK0G2EdC4YlInTgKAEHMJPc5G%2BdQvyC1v6aK%2B8blrhE9ZbwMW6FxSUYxxwzb2MQ11Cfdz0eAErqOkD6pkRU2HY%2F4iIduZd%2FtG3WmhcLFx68GI19JPPyy3Oc8ur1D6w%2BkPBcrieFJwMvHU8SL%2Bb%2BoDgE3DGvdTViCGwr5tN%2BULdOTyg6eixDsiZo5Hd0Vz0c75ysXN2z2bEevx1oC%2B1tmvGXGT%2FczI7TEBYrDY9eLHyAwIAiFCTCpKM2bCOuRf9NloxK24gityex2uOwE5N5YY05fA8ra3Nzzb6iFhZiLNmvze8z4MScrR4LADOu79275Jdd6gHBGDeS6xXzSQZJZwgCqGUJMMb6ix0Ua0kltYLFJNjkkb21sh4gTSMrw7A2oGM69C5h%2BUD8ixYQAouhODtibx%2F2NTeLnA%2B0ZkVhNzv0wud3%2FxwY6pgECFfLayoZeQp1GUobVLIVXhazi6K%2Bc5H7HQP8lOtbCr20plL0F0UCpifRaP8KfcJ485rfLKLOxq62OKznFof7yqB4Rqm8%2FEIY8bQwOsCz7o1gGXVRU6AMnja3sV7yDpjKmQQ28ZfAzPtKVGguI%2BhXUMNe%2F%2BPMdL7yNtdV0Dk3vFALU2fpXHw6mWkNR9eHpUq%2FKpnNyaRh461jL9f0NoMi%2FhbWmAM3t&X-Amz-Signature=8f7bed913348e909228c39021b22f9ad81f2a277c376eeee77afaabbbbb82424&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

